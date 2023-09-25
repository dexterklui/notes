---
title: CSS
---

# Basics

## CSS Syntax

```css
selector {
  property: value;
  another-property: value;
}
```

An example:

```css
header {
  max-width: 1080px;
  color: white;
}
```

## Selectors

- `*`
- `h1`: html element
- `#id`: id
- `.class`: class
- `h1, h2`: Multiple html element
- `img.cover-img`: element with a class
- `article > h3`: an element which is a child of a parent

## Variables

### Declaration

Variables are local to its block. Unless it is declared in `:root {}`, such
variables are globally accessible.

```css
:root {
  --blue: #1e90ff;
  --white: #ffffff;
}
```

### Usage

```css
button {
  background-color: var(--white);
}
```

## Priority and Specificity

For multiple identical _type_ of rules, the last rule takes priority. Unless
there's `!important`:

```css
p {
  color: red !important;
}
p {
  color: blue;
}
```

But when the types of rules are different, different types have different
priority. E.g. rules in id selectors takes precedence over class selectors.

Actually there is a rule of caculating **_specificity value_** that decies which
setting is prioritised. Google it.

- E.g. two class selectors add 2 times 10 points in specificity value.

## Box Model 🔲

From center to out:

- Content
- Padding
- Border
- Margin
  - I think margin of adjacent elements can overlap. Effectively the bigger one
    takes effect.

By default, the size (width and height) dictates the size in the content. But
you can override that with the property:

```css
.div {
  /* default: size targets content */
  box-sizing: content-box;
  /* size targets border + padding + content */
  box-sizing: border-box;
}
```

## Inline Blocks

Normally, only block level elements respect `height` and `width` properties. If
you want an inline level element to respect these properties, you can set
`display: inline-block` for it.

## Units

- `px`: Pixel
- `rem`: Root (i.e. the whole document) font size

  - Default is 16px, so 2rem is 32px under default font-sizs.
  - `:root { font-size: 18px; }` set a different root font size
    - `* { }` also includes `:root { }`
    - `html { }` is `:root { }`

- `em`: Parent font size
- `%`: 1% of parent's width, unless it is used for the property `height`.
- `vw` and `vh`: 1% of viewport width and height
- `dvh` and `dvw`: takes into account of the collapsible address bar on mobile /
  scrollbar
- `svw` and `svh`: always assume existence of collapsible address bar and scroll
  bar, and use the smaller viewport

## Media queries

```css
p {
    font-size = 1rem;
}
@mediae only screen and (min-width: 401px) {
}
@media only screen and (minl-width: 961px) {
}
```

Usually you style based on the smallest screen as the default. Then use media
queries at the **end** of the stylesheet to override styling for wider screen.

# CSS Selectors

## Attribute selector

- Attribute **equals** selector `[name="value"]`
- Attribute **not equals** selector `[name!="value"]`
- Attribute contains **prefix** (followed by `-`) selector `[name=|"value"]`
- Attribute **contains** word (delimited by spaces) selector `[name~="value"]`
- Attribute **starts** with selector `[name^="value"]`
- Attribute **ends** with selector `[name$="value"]`

## Pseudo-classes

### nth-child

| Selector                | Description                                     |
| ----------------------- | ----------------------------------------------- |
| `:first-child`          | First child among siblings                      |
| `:last-child`           | Last child among siblings                       |
| `:nth-child(n)`         | (n)-th child among siblings                     |
| `:nth-child(odd\|even)` | Odd or even children                            |
| `:nth-child(An+B)`      | (An+B)-th children for n ∈ \[0, numOfSiblings\] |

Note that for `:nth-child()` selector, you can use `of <selector>` after the
first argument to limit what children should be included in counting. E.g. the
following code will select the odd `.food` list items:

```
ul > li:nth-child(odd of .food)
```

### nth-of-type

The following works like [nth-child](#nth-child). But the counting only include
children of the given type before among the siblings.

- `:first-of-type`
- `:last-of-type`
- `:nth-of-type()`

Also for `:nth-of-type()`, it seems you can't use `of <selector>`.

## Pseudo-objects

- `::before`: a pseudo-element as the first child
- `::after`: a pseudo-element as the last child

# Display layout

## Flex layout

### Flex box

`display: flex`: Set the flow to be using **_flex box_** model. The **_main
axis_** is the major direction where flex items are arranged, and the **_cross
axis_** is the secondary direction. Some properties for flex box model:

- `flex-direction: row|column`: Direction of the main axis
- `justify-content`: How to justify content on each line along the main axis:
  - `flex-start`
  - `flex-end`
  - `space-between`: Even space between each item
  - `space-evenly`: Even space between each item and between item and the
    boundary of the parent
  - `center`
- `align-items`: How to align items on the same line along the cross axis
  - `flex-start`
  - `flex-end`
  - `center`
  - `stretch`: All item occupy all space available along the cross axis.
- `flex-wrap: nowrap|wrap`: Whether the items go to next line when there is not
  enough space. Default is `nowrap`, and item keep shrinking.
- `gap: <row-gap> <column-gap>`: Gap between items along rows and columns. If
  only one value, it applies to both rows and columns.

### Flex items

By default, each item has _intrinsic sizing_, meaning they only take up space of
their content.

- `align-self`: Like in `align-items`, but target this item only.
- `flex`: Shorthand property for
  - `flex-grow: 1` grow factor to fill available space in main axis
  - `flex-shrink: 1` shrink factor to fit into available space in main axis
  - `flex-basis: 33%` base size from which item grow/shrink

For some properties, e.g. `flex`, you may want to apply to all children with
`.flex-container > *` selector.

### Use cases

For flexible layout where the layout adapts to fit the content:

- Navigation links
- Tags

### Links

- [Flexbox Froggy](https://flexboxfroggy.com): A game to learn flex box basics
- [\[YouTube\] Flexbox or grid = How to decide?](https://youtu.be/3elGSZSWTbM)
- [\[YouTube\] Learn flexbox the easy way](https://youtu.be/u044iM9xsWU)

## Grid layout

### Grid

`display:grid` to make this element into a grid layout.

- `grid-auto-flow: row|column`: Flow elements inside a cell into row/columns
- [`grid-template`](#grid-template): Shorthand property for:
  - [`grid-template-areas`](#grid-template-areas) specifies named grid areas
  - `grid-template-columns` defines the width of each column; space delimited
  - `grid-template-rows` defines the height of each row; space delimited

#### grid-template

```css
.grid-container {
  grid-template:
    "a a a" 40px
    "b c c" 40px
    "b c c" 40px / 20% 4em 1fr;
}
```

#### grid-template-areas

Specifies named grid areas, establishing the cells in the grid and assigning
them names. Grid items can be assigned to a named area with `grid-area: name`.

```css
.grid-container {
  grid-template-areas:
    "a a a"
    "b c c"
    "b c c";
}

// To leave some area empty, just don't assign items. Conventionally `.` is used
// to represent empty area, and when they don't form a rectangle, they are left
// blank whatsoever.
.grid-container-two {
  grid-template-areas:
    "a a ."
    "a a ."
    ". b c";
}
```

Note that each area must be a **rectangle**.

### Grid items

- `grid-area` specifies the area occupied. Shorthand property for
  - `grid-row-start`: start from nth border (1-based / -1-based)
  - `grid-column-start`: start from nth border
  - `grid-row-end`: until nth border
  - `grid-column-end`: until nth border
  - You can have starting border on the right and ending border on the left

### Grid auto flow

Property `grid-auto-flow` control how auto-placed items get inserted in grid:

| Value          | Description                                |
| -------------- | ------------------------------------------ |
| `row`          | Default; place items by filling each row   |
| `column`       | By filling each column                     |
| `dense`        | Place items to fill any holes              |
| `row dense`    | By filling each row, and fill any holes    |
| `column dense` | By filling each column, and fill any holes |

### Units for grid template

- `fr`: Divide remaining space and share among rows/columns using `fr` using
  their individual weighting relative to their sum
  - `3fr 2fr` divides remaining space into 5 parts, first get space worth of 3
    parts, second get 2 parts

### Functions

- `repeat(4, 1fr)` expands to `1fr 1fr 1fr 1fr`.
- `repeat(auto-fit, minmax(300px, 1fr))`
- `minmax(min, max)`. If max is smaller than min, then max is ignored. `auto`
  represents the largest `max-content` size of the items.

### Use cases

For a fixed layout where the content adapts to fit the layout:

- Grid of cards

### Links

- [Grid Garden](https://cssgridgarden.com): A game to learn grid basics
- [\[YouTube\] Learn CSS Grid the easy way](https://youtu.be/rg7Fvvl3taU)
- [\[YouTube\] Learn flexbox the easy way](https://youtu.be/u044iM9xsWU)

# Topics

## Geometry

### Calculation functions

- `min(25vw, 200px)`
- `max(14px, 1em)`
- `calc(5% - 2px)`

### Resizing

To make an element resizeable by the user:

```
overflow: auto;
resize: [vertical|horizontal|both]
```

### Aspect Ratio

#### Using aspect ratio

`aspect-ratio: 16 / 9`

Note that flexbox doesn't respect it unless you also use
`overflow: (hidden|scroll)`. The idea is that if there is content overflowing,
then the aspect ratio is not respected.

#### Using padding-top

To have a cover or banner with a fixed aspect ratio, and potentially a
background, and to have content inside such as a caption or texts:

```css
.container {
  position: relative; /* required for absolute position child */
  width: 100%;
  padding-top: 56.25%; /* 16:9 aspect ratio */
  background: url("path/to/bg.jpg") no-repeat center center;
  background-size: 100% auto; /* full width, and auto height */
}
.container > .content {
  position: absolute; /* at the first relative position parent */
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
```

### Shape clipping

`clip-path` property clips an element into given shape using builtin functions
or svg paths.

## Colour

### Colour keywords

- [Named colour](https://developer.mozilla.org/en-US/docs/Web/CSS/named-color)
- [System colour](https://developer.mozilla.org/en-US/docs/Web/CSS/system-color)
- `currentcolor` takes the value of an element's `color` property. If used on
  the `color` property, it takes the value from the **inherited** value of the
  `color` property.

### Colour functions

#### Picking particular colour

All of the following function can accept an optional forth argument separated by
a `/` for the alpha-value (opacity): `rgb(255 255 255 / .5)`.

- `rgb(255 0 153)`: red (0-255), green (0-255), blue (0-255)
- `hsl(150, 30%, 60%)`: hue (0-360), saturation (0-100%), lightness (0-100%)
  - hue: 0-red, 120-green, 240-blue
- `hwb(194 50% 0%)`: hue, whiteness, blackness
- `lab(50% 40 59.5)`: lightness, A-axis, B-axis

`none` can be used for any argument. It is used for colour interpolation.
Otherwise it is effectively **0**.

#### Mixing colours

`color-mix()` takes two `color` values and return the result of mixing them in a
given colour space by a given amount.

See
[mdn web docs](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/color-mix).

## Gradient colour

### Gradient is an image

Note that gradient colour belongs to `<image>` data type. So it won't work on
`background-color` and other properties that use `<color>` data type. Use it on
`background` or `background-image`.

### Linear colour gradient

#### Syntax

`linear-gradient([orientation], colour-point...)`

#### Orientation

- Given angle is counted clockwise from the orientation bottom-to-top.
- Unit includes `deg`, and `turn` (1 turn = 360 deg)
- Can give textual instruction instead, e.g. `to left top`, `to bottom`
- Default is top to bottom, i.e. `180deg` / `to bottom` / `0.5turn`.

#### Colour points

- Syntax: `colour [start-pos] [end-pos]`

- You can skip both pos, or just skip end-pos

- Pos are given in distance units, like %, em, px.

- Colour changes smoothly between colour points.

- In case of an overlapping range, former colour has priority, creating a hard
  transition.

- If no pos is provided, CSS will try to even out the distance between colour
  points.

#### Multiple linear-gradients

You can apply **multiple** linear-gradient to blend colours from different
direction. You may want to use alpha-value in this case to make colour less
opaque at the far end.

The following is a beautiful blending of three original colours.

```
background: linear-gradient(217deg, rgba(255,0,0,.8), rgba(255,0,0,0) 70.71%),
            linear-gradient(127deg, rgba(0,255,0,.8), rgba(0,255,0,0) 70.71%),
            linear-gradient(336deg, rgba(0,0,255,.8), rgba(0,0,255,0) 70.71%);
```

#### Links

[Linear gradient - mdn web docs](https://developer.mozilla.org/en-US/docs/Web/CSS/gradient/linear-gradient)

### Repeating conic gradient

```
background: repeating-conic-gradient(
  from 45deg,
  red 0,
  yellow 15%,
  red 33%
);
```

- Gradient transition from red at 12 o'clock to yellow at 15% revolution.
- Then to red at 33% revolution.
- Then repeat (red at 33% to yellow at 48%...)
- The whole result rotated clockwise by 45 degrees.

---

```
background: repeating-conic-gradient(
  from 45deg,
  black 0 90deg,
  white 90deg 180deg,
)
```

This is a 45 degree tilted black-and-white checker board pattern:

- Solid black from 0 to 90 degrees.
- Solid white from 90 degrees to 180 degrees.
- Repeat...
- The whole result rotated clockwise by 45 degrees.

---

```
background: repeating-conic-gradient(
  from 0deg at 25% 50%,
  black 0 180deg,
  white 180deg 360deg,
)
```

This is just left 25% white right 75% black. The `at 25% 50%` place the centre
of rotation at 25% width from the left and 50% height from the top.

### Other gradient functions

- `repeating-linear-gradient()`
- `conic-gradient()`
- `radial-gradient()`
- `repeating-radial-gradient()`

### Gradient in text

1.  Set up colour gradient in the background
2.  **Mask** the background to the text
    - `background-clip: text`
    - `-webkit-background-clip: text` for chrome
3.  Set the text to be transparent
    - `color: transparent`

## Background

### Background fill

`background-size: {cover|fill|contain}`

### Background blend-mode

Blend the background picture with background colour.

`background-blend-mode`:

- [list of values](https://css-tricks.com/almanac/properties/b/background-blend-mode/)
- `multiply`

## Positioning

- `position`:
  - `relative`:
    - The position the element originally should have occupied is still
      considered occipied.
    - The element move relative to it's own original position.
  - `absolute`:
    - The position the element originally should have occupied is not occupied,
      no mattered it is moved or not. That is, the element is as if
      **non-existent in HTML flow**.
    - The element move relative to it's first ancestors that is in `relative`
      position. Furthest is the `<html>` tag.

## Text

### Text wrap

- `white-space: nowrap`

### Spacing

- `letter-spacing`

### Truncate

```css
p {
  display: -webkit-box;
  -webkit-line-clamp: 3; /* limit contents to 3 lines */
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

But I guess I could just use traditional CSS. Like `height: 3em`, instead of
webkit.

### Gradient colour

See [Gradient in text](#gradient-in-text)

### Writing direction

`writing-mode` property sets the orientation of text flow and direction of block
grow.

- `horizontal-tb` or `horizontal-bt`: text flows from left to right
- `vertical-rl` or `vertical-lr`: text turns 90% clockwise and flows from top to
  bottom
- Some more
  [experimental values...](https://developer.mozilla.org/en-US/docs/Web/CSS/writing-mode#values)

### Typewriter effect

See [Typewriter animations](#typewriter-animations).

## Images

### Fill mode

`object-fit: {cover|fill|contain}`

For `object-fit` to work, the image needs a width and height. Usually you want
to be 100% and 100%. These specifies the **bounding box** of the image, and then
the `object-fit` is functioning with respect to this bounding box.

- cover: zoom to fill space, maintaining aspect ratio
- contained: zoom to see the whole image, maintaining aspect ratio

### Flipping

`transform: scaleX(-1)` or `scaleY(-1)`.

### Misc

- [Aspect Ratio](#aspect-ratio)

## Events and interaction

### pointer-events

Attribute `pointer-events` specifies under which circumstances an element can
become the target of pointer events. It can be useful to prevent user from
selecting an element (e.g. drag-and-drop) or highlighting the text, for e.g.
aesthetic purposes.

| Value  | Description                                    |
| ------ | ---------------------------------------------- |
| `auto` | Default.                                       |
| `none` | Never; but event can trigger during            |
|        | capture/bubble phase when descendant is target |

There are
[SVG-only values](https://developer.mozilla.org/en-US/docs/Web/CSS/pointer-events#values).

## Animation

### Transition

- `transition` is a shorthand property for:
  - `transition-property`: CSS property, `all`, or `none` (default)
  - `transition-duration`: In `s` or `ms`
  - `transition-timing-function`: like
    [`animation-timing-function`](#animation-timing-function)
  - `transition-delay`: In `s` or `ms`

All properties can accept a **comma-delimited** list of values, to specify
different transition styles for different CSS styles.

```
transition:
  margin-right 4s ease-out;
  color 2s ease;
```

### Animation definition

Use `@keyframes` directives:

```css
@keyframes my-animation {
  0%,
  100% {
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
}
```

### Animation properties

#### animation property

A shorthand property for:

- `animation-name`
- `animation-duration`
- `animation-timing-function`
- `animation-delay`
- `animation-iteration-count`
- `animation-direction`
- `animation-fill-mode`
- `animation-play-state`

#### animation-duration

Default is 0. Units can be `s` or `ms`.

#### animation-timing-function

| Value                    | Description                                     |
| ------------------------ | ----------------------------------------------- |
| `linear`                 | Same speed from start to end                    |
| `ease`                   | Default; Slow start, then fast, then slow end   |
| `ease-in`                | Slow start                                      |
| `ease-out`               | Slow end                                        |
| `ease-in-out`            | Slow start and end                              |
| `step-start`             | Like `steps(1, start)`                          |
| `step-end`               | Like `steps(1, end)`                            |
| `steps(n, [start\|end])` | No. of intervals, change at start/end (default) |

Usually `ease-out` is the best. `ease-in` is too slow in the start making user
feel slack and unresponsive. `linear` stops too abruptly.

#### animation-delay

Default is 0. Units can be `s` or `ms`. **Negative** values are allowed, and it
means the animation will start as if it had already been playing for that
absolute amount of time.

#### animation-iteration-count

A number (default is 1) or `infinite`

#### animation-direction

| Value               | Description                    |
| ------------------- | ------------------------------ |
| `normal`            | Default; forwards              |
| `reverse`           | Backwards                      |
| `alternate`         | Forwards first, then backwards |
| `alternate-reverse` | Backwards first, then forwards |

Animation speed for alternate remains the same (so effectively doubling the
animation duration for each iteration).

#### animation-fill-state

Specify how animation affects the styles before and after the animation.

| Value       | Description                            |
| ----------- | -------------------------------------- |
| `none`      | Default, no animation style applied    |
| `forwards`  | Style of last keyframe remains after   |
| `backwards` | Style of first keyframe applies before |
| `both`      | Both `forwards` and `backwards`        |

#### animation-play-state

| Value     | Description                                    |
| --------- | ---------------------------------------------- |
| `paused`  | Animation for this element is paused           |
| `running` | Default; animation for this element is running |

#### animation-range

_Experimental_

Shorthand property for `animation-range-start` and `animation-range-end`
properties.

They specify where along the timeline an animation will start or end. Values:

- `normal`: default
- Percentages
- And
  [more...](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-range#values)

#### animation-timeline

_Experimental_ (Only Chrome supports)

| Value    | Description                                                 |
| -------- | ----------------------------------------------------------- |
| `auto`   | Default; By passing of time since document was first loaded |
| `view()` | By visibility of an element inside the nearest scroller     |

And
[more...](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-timeline#values)

### Using multiple animations

You can apply multiple animations on a single element.

```
animation:
  typing 3.5s steps(40, end),
  blink-caret .75s step-end infinite;
```

### Examples

#### Typewriter animations

In this example the animation is as following ("|" is the blinking caret):

1.  | Project
2.  **My early** | Project
3.  | Project
4.  **My very armature** | project
5.  | Project
6.  (start all over again)

```html
<h2>Project</h2>
```

```css
h2::before {
  content: ""; /* need to declare here for animation to change it */
  display: inline-block; /* for width property to take effect */
  white-space: nowrap; /* don't wrap text */
  overflow-x: hidden; /* don't show text beyond the width */
  animation: projects-typewriter 6s infinite both linear, blinking-caret 0.5s
      infinite;
  border-right: 0.1em solid orange; /* this is the blinking caret */
  position: relative;
  top: 0.15em; /* trying to fix the displacement from inline-block display */
  margin-right: 0.2em;
  color: orange;
}

@keyframes projects-typewriter {
  0% {
    content: "My early";
    width: 0;
  }
  10% {
    /* content doesn't have interpolation. If you don't specify, it is empty. */
    content: "My early";
    width: 8rem;
  }
  35% {
    content: "My early";
    width: 8rem;
  }
  40% {
    content: "My very armature";
    width: 0;
  }
  43% {
    content: "My very armature";
    width: 0;
  }
  60% {
    content: "My very armature";
    width: 16rem;
  }
  90% {
    content: "My very armature";
    width: 16rem;
  }
  97% {
    content: "My very armature";
    width: 0;
  }
  100% {
    content: "My very armature";
    width: 0;
  }
}

@keyframes blinking-caret {
  0%,
  100% {
    border-right-color: transparent;
  }
  50% {
    border-right-color: orange;
  }
}
```

## Cursor

`cursor` attribute controls cursor style hovering on the element. Actual cursor
icons are dependent on platform and user's computer setting.

### General cursors

| Value     | Description                             |
| --------- | --------------------------------------- |
| `auto`    | **Default**, UA determine automatically |
| `default` | Platform-dependent default cursor       |
| `none`    | No cursor is rendered                   |

### Links and status cursors

| Value          | Description                                   |
| -------------- | --------------------------------------------- |
| `context-menu` | Context menu available                        |
| `help`         | Help info available                           |
| `pointer`      | Index finger to indicate a **link**           |
| `progress`     | Program is busy in bg, but can still interact |
| `wait`         | Program is busy, cannot interact              |

### Selection cursors

| Value           | Description                                  |
| --------------- | -------------------------------------------- |
| `cell`          | Like Excel                                   |
| `crosshair`     | Often used to indicate selection in a bitmap |
| `text`          | Text can be selected                         |
| `vertical-text` | Vertical text can be selected                |

### Drag and drop cursors

| Value         | Description                |
| ------------- | -------------------------- |
| `alias`       | Creating alias or shortcut |
| `copy`        | Copying something          |
| `move`        | Moving something           |
| `no-drop`     | Cannot be dropped here     |
| `not-allowed` |                            |
| `grab`        | Something can be grabbed   |
| `grabbing`    | Grabbing something         |

### Resize and scroll cursors

For resizing and scrolling (arrows), the value is
`<direction>[direction]-resize`, where direction is given by `n`, `e`, `s`, `w`,
`ne`, etc.

### Zoom cursors

| Value      | Description |
| ---------- | ----------- |
| `zoom-in`  | Zoom in     |
| `zoom-out` | Zoom out    |

## Html tables

- `padding` of `<td>` controls distance between content and border of the cell.
- `border-spacing` of `<table>` controls space between border of the cells.
  - Make sure `border-collapse: separate` but not `border-collapse: collapse`
    for `border-spacing` to work on `<table>`
  - In tailwind, it seems the default changes to `collapse`

# Queries

## Query conditions

### Logical keywords

- `and`
- `or`
- `not`: only one `not` is allowed and cannot be used with `and` or `or`

### Condition

- `(max-width: 500px)`
- `(min-height: 200px)`
- `(width >= 1080px)`
- `(min-width: 640px) and (max-width: 1280px)`

## Container queries

`@container` queries can apply CSS styling based on ancestor element's size.

### Quick example and syntax

```css
@container (width <= 250px) {
  .btn {
    width: 50px;
    fonr-size: 0.6rem;
  }
}
```

```css
@container [<container-name>] <container-condition> {
  /* css styling */
}
```

### container

`container: <name> / <type>` is a short hand attribute.

### container-type

Parent has set `container-type` to be either `inline-size` or `size`

- `inline-size`: query based on **inline** dimension
- `size`: based on both **inline** and **block** dimension

Then you can use container query:

### container-name

You can also give the container a case-sensitive name using `container-name`
which can be used as a filters in later container queries:
`@container my-container (width > 500px)`.

### Container conditions

- `width`
- `height`
- `aspect-ratio`
- `block-size`
- `inline-size`
- `orientation`: `landscape` or `portrait`

# Tips

## Apply a filter

## Translate position

Besides using `position` property, you can use `transform` property with
`translateX()` and `translateY()` function. For example, you vertically align
some text in a `<p>`, but you can't use

```css
p.text {
  display: table-cell;
  vertical-align: middle;
}
```

Then you can do:

```css
p.text {
  transform: translateY(calc(50% - 1em));
}
```

## One-liners

### Width

`width: clamp(280px, 60%, 1200px)` is equivalent to

```
width: 60%;
min-width: 280px;
max-width: 1200px;
```

You can nest other functions for each of the three values:

- `min()`
- `max()`
- `calc()`

## CSS Grids Layouts

### Header and footer

```css
.main-layout {
  // can be body
  min-height: 100vh; // as fallback if browser doesn't understand dvh
  min-height: 100dvh; // dvh takes into account mobile's collapsable address bar

  display: grid;
  grid-template-rows:
    auto // for header
    1fr
    auto; // for footer
  // auto means they take only the space of their content.
}
```

### Stacking content

```css
.primary-header {
  display: grid;
  grid-template-areas: "stack";
  place-items: center;
}
.primary-header > * {
  grid-area: stack;
}
```

Alternative method:

```css
.primary-header {
  display: grid;
  place-items: center;
}
.primary-header > * {
  grid-area: 1 / 1 / -1 / -1; // row-s, col-s, row-e, col-e
}
```

The second method has an advantage to change layout with media queries.

### Stretching out children

Children of a grid would automatically have their size stretched out to fill the
cell. So instead of setting up each different type of elements with
`width: 100%`, you can just do `display: grid`.

## Quotes pseudo-element

```css
::bofre {
  content: open-quote;
}
```

The next open-quote becomes a single quote. To tell CSS not to nest quotes, you
need to close quotes at the end (e.g. at `::after`) with `close-quote` or
`no-close-quote`.

## Isolation

```css
.isolation {
  isolation: isolate;
}
```

Creates a isolated stacking context for `z-index` so children's z-index won't
affect their relative z position to elements outside the parent.

## Inset

```css
inset: auto 1px 2rem 3em;
// is identical to
top: auto;
right: 1px;
bottom: 2rem;
left: 3em;
```

## Counters

```css
section {
  counter-increment: my-section-counter;
}
.section-title::before {
  content: counter(my-section-counter) ". ";
  opacity: 0.5;
}
```

## CSS Contain

`contain` property makes certain aspect of CSS styling contained within an
element (no effect outside). Some values:

- `style` contains the `counter-increment` and apply scope to the counter
  variable
- `paint` / `layout` contains layout recalculations, so that, say, animation
  within won't cause outside elements to readjust.

### Example: style

In above counter example, counter increments after each section. If you want to
insert a contained section having its own counter:

```css
.contain-section {
  contain: style;
}
.contain-section .section-title {
  counter-increment: my-section-counter;
}
```

Now the counting outside won't take `.contain-section` into account (not sure),
and within it there is a new counting from 1 for each .section-title.

## Filters and focus-within

```css
.card__img {
  filter: grayscale(100%) contrast(200%);
  transition: filter 500ms ease;
}
.card:hover > .card__img,
.card:focus-within > .card__img {
  filter: grayscale(0%) contrast(100%);
}
```

## Invert colour

```css
@media (perfers-color-scheme: dark) {
  .icon {
    filter: invert(100%);
  }
}
```

## Shadow

```css
// Shadow only appears outside the border box.
box-shadow: 0px 0px 1rem purple;
// Can see throguh invisible part of the element to the shadow
filter: drop-shadow(0px 0px 1rem red);
```

# 🧭 Navigation

- [🔼 Back to top](#)
- [📑 Notes Index](../../index.md)
- [🗃️ Index](/media/mikeX/Nextcloud/index.md)
