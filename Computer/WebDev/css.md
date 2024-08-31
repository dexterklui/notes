# CSS

## Basics

### CSS Syntax

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

### Selectors

- `*`
- `h1`: html element
- `#id`: id
- `.class`: class
- `h1, h2`: Multiple html element
- `img.cover-img`: element with a class
- `article > h3`: an element which is a child of a parent

### Variables

#### Declaration

Variables are local to its block. Unless it is declared in `:root {}`, such
variables are globally accessible.

```css
:root {
  --blue: #1e90ff;
  --white: #ffffff;
}
```

#### Usage

```css
button {
  background-color: var(--white);
}
```

### Priority and Specificity

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

Actually there is a rule of calculating **_specificity value_** that decides
which setting is prioritised. Google it.

- E.g. two class selectors add 2 times 10 points in specificity value.

### Box Model 🔲

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

### Inline Blocks

Normally, only block level elements respect `height` and `width` properties. If
you want an inline level element to respect these properties, you can set
`display: inline-block` for it.

### Units

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

### Media queries

```css
p {
    font-size = 1rem;
}
@media only screen and (min-width: 401px) {
}
@media only screen and (minl-width: 961px) {
}
```

Usually you style based on the smallest screen as the default. Then use media
queries at the **end** of the stylesheet to override styling for wider screen.

### Font

Use `font-family` property. It is automatically inherited by child element in
most cases, except for perhaps `input`, `textarea` and maybe `button` too on
mobile devices.

## CSS Selectors

### Attribute selector

- Attribute **equals** selector `[name="value"]`
- Attribute **not equals** selector `[name!="value"]`
- Attribute contains **prefix** (followed by `-`) selector `[name=|"value"]`
- Attribute **contains** word (delimited by spaces) selector `[name~="value"]`
- Attribute **starts** with selector `[name^="value"]`
- Attribute **ends** with selector `[name$="value"]`

### Pseudo-classes

#### nth-child

| Selector                | Description                                |
| ----------------------- | ------------------------------------------ |
| `:first-child`          | First child among siblings                 |
| `:last-child`           | Last child among siblings                  |
| `:nth-child(n)`         | (n)-th child among siblings                |
| `:nth-child(odd\|even)` | Odd or even children                       |
| `:nth-child(An+B)`      | (An+B)-th children for n ∈ \[0, infinity\] |

Note that for `:nth-child()` selector, you can use `of <selector>` after the
first argument to limit what children should be included in counting. E.g. the
following code will select the odd `.food` list items:

```css
ul > li:nth-child(odd of .food)
```

#### nth-of-type

The following works like [nth-child](#nth-child). But the counting only include
children of the given type before among the siblings.

- `:first-of-type`
- `:last-of-type`
- `:nth-of-type()`

Also for `:nth-of-type()`, it seems you can't use `of <selector>`.

### Pseudo-objects

- `::before`: a pseudo-element as the first child
- `::after`: a pseudo-element as the last child

### Specificity

- Put queries into `:where()` to make the specificity zero. Useful for defining
  default style for easy overriding.
- `html` is less specific than `:root`.
- You can **repeat** selector to increase specificity, e.g. `.btn.btn`.

## Display layout

### Flex layout

#### Flex box

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
- `align-content`: Determine spacing between each line. Like justify-content but
  for lines along the cross axis.
  - `flex-start`
  - `flex-end`
  - `center`
  - `space-between`
  - `space-around`
  - `stretch`: lines are stretched to fit the container
- `flex-wrap: nowrap|wrap|wrap-reverse`: Whether the items go to next line when
  there is not enough space. `wrap-reverse` makes line wrap to reverse
  direction, effectively reversing the direction of cross-axis. Default is
  `nowrap`, and item keep shrinking.
- `gap: <row-gap> <column-gap>`: Gap between items along rows and columns. If
  only one value, it applies to both rows and columns.
- `flex-flow`: Short form for `flex-direction` and `flex-wrap`.

#### Flex items

By default, each item has _intrinsic sizing_, meaning they only take up space of
their content.

- `align-self`: Like in `align-items`, but target this item only.
- `margin: auto`: Make this item occupy all space available along the main axis
  by pushing adjacent items to the side.
- `flex`: Shorthand property for
  - `flex-grow: 1` grow factor to fill available space in main axis
  - `flex-shrink: 1` shrink factor to fit into available space in main axis
  - `flex-basis: 33%` base size from which item grow/shrink
- `order`: change the ordering among flex items. Can be any integer. Default is
  0, smaller order ranks earlier than larger order.

For some properties, e.g. `flex`, you may want to apply to all children with
`.flex-container > *` selector.

#### Use cases

For flexible layout where the layout adapts to fit the content:

- Navigation links
- Tags

#### With margins

Using `margin: auto` on a flex item will make it occupy all space available,
i.e. pushing adjacent items to the side. E.g. `margin-right: auto` makes all the
following flex items to `flex-end`.

#### Links

- [Flexbox Froggy](https://flexboxfroggy.com): A game to learn flex box basics
- [\[YouTube\] Flexbox or grid = How to decide?](https://youtu.be/3elGSZSWTbM)
- [\[YouTube\] Learn flexbox the easy way](https://youtu.be/u044iM9xsWU)

### Grid layout

#### Grid

`display:grid` to make this element into a grid layout.

- `grid-auto-flow: row|column`: Flow elements inside a cell into row/columns
- [`grid-template`](#grid-template): Shorthand property for:
  - [`grid-template-areas`](#grid-template-areas) specifies named grid areas
    (optional)
  - `grid-template-columns` defines the width of each column; space delimited
  - `grid-template-rows` defines the height of each row; space delimited
- `grid-auto-columns` and `grid-auto-rows`: Specify the size of an
  implicitly-created grid column/row (e.g. in grid-auto-flow or explicit
  out-of-range assignment in grid items). Default is `auto`.

##### grid-template

To define rows and columns width only:

```css
.grid-container {
  /* 3 rows of 40px, 3 columns of various width */
  grid-template: 40px 40px 40px / 20% 4em 1fr;
}
```

To define named grid areas:

```css
.grid-container {
  grid-template:
    "a a a" 40px
    "b c c" 40px
    "b c c" 40px / 20% 4em 1fr;
}
```

##### grid-template-areas

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

#### Grid items

- `grid-row-start`: start from nth border
  - Integer (1-based / -1-based from the right): until nth border. Default is
    value of `grid-row-end` - 1.
  - `span 2` to span two rows.
- `grid-row-end`:
  - Integer: until nth border. Default is value of `grid-row-start` + 1. You can
    have starting border on the right and ending border on the left
  - `span 2` to span two rows.
- `grid-column-start`: similar as above
- `grid-column-end`: similar as above
- `grid-column: 2 / 4`: shorthand for `grid-column-start` and `grid-colun-end`
- `grid-row`: similar as above
- `grid-area: 1 / 1 / 3 / 6` specifies the area occupied. Shorthand property for
  `grid-row-start`, `grid-column-start`, `grid-row-end`, `grid-column-end`.
  - You can overlap grid areas.
- `order`: works like order in flexbox

#### Grid auto flow

Property `grid-auto-flow` control how auto-placed items get inserted in grid:

| Value          | Description                                |
| -------------- | ------------------------------------------ |
| `row`          | Default; place items by filling each row   |
| `column`       | By filling each column                     |
| `dense`        | Place items to fill any holes              |
| `row dense`    | By filling each row, and fill any holes    |
| `column dense` | By filling each column, and fill any holes |

#### Named grid lines

You can name grid lines and specified grid line names can automatically name
grid column areas. This method is especially useful to define a content grid
with normal width, breakout-width, full-width areas to rid the need of making
wrapper divs.

Use square brackets to name grid lines. A grid line name must end with `-start`
or `-end` so as to designate their corresponding grid area.

```css
.content-grid {
  display: grid;
  grid-template-columns:
    [full-width-start] 1fr
    [breakout-start] 1fr
    [content-start] 1fr
    [content-end] 1fr
    [breakout-end] 1fr
    [full-width-end];
}

.content-grid > * {
  grid-column: content; /* between content-start and content-end */
}

.content-grid > .breakout {
  grid-column: breakout; /* between breakout-start and breakout-end */
}

.content-grid > .full-width {
  grid-column: full-width; /* between full-width-start and full-width-end */
}
```

##### Responsive and improved column width

```css
.content-grid,
.full-width {
  /* recursively full-width also is a content-grid, so its children also reside
   * in the same boundary
   */
  --padding-inline: 2rem;
  --breakout-max-width: 85ch;
  --content-max-width: 70ch;
  --breakout-column-max-width: calc(
    (var(--breakout-max-width) - var(--content-max-width)) / 2
  );

  display: grid;
  grid-template-columns:
    [full-width-start] minmax(var(--padding-inline), 1fr)
    [breakout-start] minmax(0, var(--breakout-column-max-width))
    [content-start] min(
      var(--content-max-width),
      100% - 2 * var(--padding-inline)
    )
    [content-end] minmax(0, var(--breakout-column-max-width))
    [breakout-end] minmax(var(--padding-inline), 1fr)
    [full-width-end];
}

.content-grid > :not(.breakout, .full-width),
.full-width > :not(.breakout, .full-width) {
  grid-column: content;
}

.content-grid > .breakout,
.full-width > .breakout {
  grid-column: breakout;
}

.content-grid > .full-width {
  grid-column: full-width;
}
```

Reference: this [YouTube video](https://www.youtube.com/watch?v=c13gpBrnGEw).

#### Units for grid template

- `fr`: Divide remaining space and share among rows/columns using `fr` using
  their individual weighting relative to their sum
  - `3fr 2fr` divides remaining space into 5 parts, first get space worth of 3
    parts, second get 2 parts

#### Functions for grid-layout

See
[A Deep Dive Into CSS Grid minmax](https://ishadeed.com/article/css-grid-minmax/)

- `repeat(4, 1fr)` expands to `1fr 1fr 1fr 1fr`.
- `repeat(auto-fit, minmax(300px, 1fr))`
  - `1fr` means one share of remaining available space (i.e. after allocating
    space to other columns/rows with other units, the remaining space are shared
    among the columns/rows specified with the unit `fr`). Can be `0fr`.
  - `auto-fit` tries to fit as many column as possible. It expands the grid
    items to fill the available space.
  - `auto-fill` tries to fill up the full row width. It won't expands the grid
    items, and remaining space are left unfilled.

##### minmax

In grid, usually items automatically shrink to fit all items or expand to fill
up available space. `minmax(min, max)` is used to define a size range.

- `auto` represents the largest `max-content` size of the items.

Invalid inputs:

- If `max` is smaller than `min`, then `max` is ignored.
- If `min` is `1fr`, then whole declaration is ignored. `1fr` is only valid for
  `max`

#### Use cases

For a fixed layout where the content adapts to fit the layout:

- Grid of cards

```css
.wrapper {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(min(100%, 250px), 1fr));
  grid-gap: 1rem;
}
```

#### Links

- [Grid Garden](https://cssgridgarden.com): A game to learn grid basics
- [\[YouTube\] Learn CSS Grid the easy way](https://youtu.be/rg7Fvvl3taU)
- [\[YouTube\] Learn flexbox the easy way](https://youtu.be/u044iM9xsWU)

## Topics

### Geometry

#### Calculation functions

- `min(25vw, 200px)`
- `max(14px, 1em)`
- `calc(5% - 2px)`

#### Resizing

To make an element resizeable by the user:

```text
overflow: auto;
resize: [vertical|horizontal|both]
```

#### Aspect Ratio

##### Using aspect ratio

`aspect-ratio: 16 / 9`

Note that flexbox doesn't respect it unless you also use
`overflow: (hidden|scroll)`. The idea is that if there is content overflowing,
then the aspect ratio is not respected.

##### Using padding-top

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

#### Shape clipping

`clip-path` property clips an element into given shape using builtin functions
or svg paths.

### Colour

#### Colour keywords

- [Named colour](https://developer.mozilla.org/en-US/docs/Web/CSS/named-color)
- [System colour](https://developer.mozilla.org/en-US/docs/Web/CSS/system-color)
- `currentcolor` takes the value of an element's `color` property. If used on
  the `color` property, it takes the value from the **inherited** value of the
  `color` property.

#### Colour functions

##### Picking particular colour

All of the following function can accept an optional forth argument separated by
a `/` for the alpha-value (opacity): `rgb(255 255 255 / .5)`.

- `rgb(255 0 153)`: red (0-255), green (0-255), blue (0-255)
- `hsl(150, 30%, 60%)`: hue (0-360), saturation (0-100%), lightness (0-100%)
  - hue: 0-red, 120-green, 240-blue
- `hwb(194 50% 0%)`: hue, whiteness, blackness
- `lab(50% 40 59.5)`: lightness, A-axis, B-axis

`none` can be used for any argument. It is used for colour interpolation.
Otherwise it is effectively **0**.

##### Mixing colours

`color-mix()` takes two `color` values and return the result of mixing them in a
given colour space by a given amount.

See
[mdn web docs](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/color-mix).

- `color-mix(in srgb, var(--primary-color) 50%, transparent)`

##### Add opacity value

Use [colours mixing](#mixing-colours) techniques.

### Gradient colour

#### Gradient is an image

Note that gradient colour belongs to `<image>` data type. So it won't work on
`background-color` and other properties that use `<color>` data type. Use it on
`background` or `background-image`.

#### Linear colour gradient

##### Syntax

`linear-gradient([orientation], colour-point...)`

##### Orientation

- Given angle is counted clockwise from the orientation bottom-to-top.
- Unit includes `deg`, and `turn` (1 turn = 360 deg)
- Can give textual instruction instead, e.g. `to left top`, `to bottom`
- Default is top to bottom, i.e. `180deg` / `to bottom` / `0.5turn`.

##### Colour points

- Syntax: `colour [start-pos] [end-pos]`

- You can skip both pos, or just skip end-pos

- Pos are given in distance units, like %, em, px.

- Colour changes smoothly between colour points.

- In case of an overlapping range, former colour has priority, creating a hard
  transition.

- If no pos is provided, CSS will try to even out the distance between colour
  points.

##### Multiple linear-gradients

You can apply **multiple** linear-gradient to blend colours from different
direction. You may want to use alpha-value in this case to make colour less
opaque at the far end.

The following is a beautiful blending of three original colours.

```
background: linear-gradient(217deg, rgba(255,0,0,.8), rgba(255,0,0,0) 70.71%),
            linear-gradient(127deg, rgba(0,255,0,.8), rgba(0,255,0,0) 70.71%),
            linear-gradient(336deg, rgba(0,0,255,.8), rgba(0,0,255,0) 70.71%);
```

##### Links

[Linear gradient - mdn web docs](https://developer.mozilla.org/en-US/docs/Web/CSS/gradient/linear-gradient)

#### Repeating conic gradient

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

#### Other gradient functions

- `repeating-linear-gradient()`
- `conic-gradient()`
- `radial-gradient()`
- `repeating-radial-gradient()`

#### Gradient in text

1. Set up colour gradient in the background
2. **Mask** the background to the text
   - `background-clip: text`
   - `-webkit-background-clip: text` for chrome
3. Set the text to be transparent
   - `color: transparent`

### Background

#### Background fill

`background-size: {cover|fill|contain}`

#### Background blend-mode

Blend the background picture with background colour.

`background-blend-mode`:

- [list of values](https://css-tricks.com/almanac/properties/b/background-blend-mode/)
- `multiply`

#### Background attachment

`background-attachment: {scroll|fixed|local}`. Default is `scroll`.

### Positioning

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

### Text

#### Text wrap

- `white-space: nowrap`

#### Spacing

- `letter-spacing`

#### Truncate

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

#### Gradient colour

See [Gradient in text](#gradient-in-text)

#### Writing direction

`writing-mode` property sets the orientation of text flow and direction of block
grow.

- `horizontal-tb` or `horizontal-bt`: text flows from left to right
- `vertical-rl` or `vertical-lr`: text turns 90% clockwise and flows from top to
  bottom
- Some more
  [experimental values...](https://developer.mozilla.org/en-US/docs/Web/CSS/writing-mode#values)

#### Typewriter effect

See [Typewriter animations](#typewriter-animations).

### Images

#### Fill mode

`object-fit: {cover|fill|contain}`

For `object-fit` to work, the image needs a width and height. Usually you want
to be 100% and 100%. These specifies the **bounding box** of the image, and then
the `object-fit` is functioning with respect to this bounding box.

- cover: zoom to fill space, maintaining aspect ratio
- contained: zoom to see the whole image, maintaining aspect ratio

#### Flipping

`transform: scaleX(-1)` or `scaleY(-1)`.

#### Misc

- [Aspect Ratio](#aspect-ratio)

### Events and interaction

#### pointer-events

Attribute `pointer-events` specifies under which circumstances an element can
become the target of pointer events.

Setting it to `none` is very useful in preventing an element from stealing click
event, allowing user to **click through** it. It can be also useful to prevent
user from selecting an element (e.g. drag-and-drop), or highlighting the text
for aesthetic purposes.

| Value  | Description                                    |
| ------ | ---------------------------------------------- |
| `auto` | Default.                                       |
| `none` | Never; but event can trigger during            |
|        | capture/bubble phase when descendant is target |

There are
[SVG-only values](https://developer.mozilla.org/en-US/docs/Web/CSS/pointer-events#values).

### Animation

#### Transition

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

##### Properties with special behaviour

###### Height

For height, you need to set fixed value for transition to work, like `rem` and
`px`, but not `100%`, `max-content`, or `auto`. If you want to have transition
effect for flexible value height, do transition on `max-height`.

#### Transition conflicts with Animation

If there is `animation` (at least with `animation-fill-state` not `none`),
transition effect will be ignored. So you need to set `animation` to none first.

Note that when synchronously setting multiple CSS properties that involve
animation or transition, the actual order of the changes isn't guaranteed. So
the following might not work:

```javascript
const myElem = document.querySelector("#my-elem");
myElem.style.setProperty("animation", "none");
myElem.style.setProperty("opacity", "0"); /* transition effect might not work */
```

You should use `requestAnimationFrame()` to make sure the changes are applied in
the next repaint. So use following instead:

```javascript
const myElem = document.querySelector("#my-elem");
myElem.style.setProperty("animation", "none");
requestAnimationFrame(() => {
  myElem.style.setProperty("opacity", "0"); /* transition effect works */
});
```

#### Animation definition

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

#### Animation properties

##### animation property

A shorthand property for:

- `animation-name`
- `animation-duration`
- `animation-timing-function`
- `animation-delay`
- `animation-iteration-count`
- `animation-direction`
- `animation-fill-mode`
- `animation-play-state`

##### animation-duration

Default is 0. Units can be `s` or `ms`.

##### animation-timing-function

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

##### animation-delay

Default is 0. Units can be `s` or `ms`. **Negative** values are allowed, and it
means the animation will start as if it had already been playing for that
absolute amount of time.

##### animation-iteration-count

A number (default is 1) or `infinite`

##### animation-direction

| Value               | Description                    |
| ------------------- | ------------------------------ |
| `normal`            | Default; forwards              |
| `reverse`           | Backwards                      |
| `alternate`         | Forwards first, then backwards |
| `alternate-reverse` | Backwards first, then forwards |

Animation speed for alternate remains the same (so effectively doubling the
animation duration for each iteration).

##### animation-fill-state

Specify how animation affects the styles before and after the animation.

| Value       | Description                            |
| ----------- | -------------------------------------- |
| `none`      | Default, no animation style applied    |
| `forwards`  | Style of last keyframe remains after   |
| `backwards` | Style of first keyframe applies before |
| `both`      | Both `forwards` and `backwards`        |

##### animation-play-state

| Value     | Description                                    |
| --------- | ---------------------------------------------- |
| `paused`  | Animation for this element is paused           |
| `running` | Default; animation for this element is running |

##### animation-range

_Experimental_

Shorthand property for `animation-range-start` and `animation-range-end`
properties.

They specify where along the timeline an animation will start or end. Values:

- `normal`: default
- Percentages
- And
  [more...](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-range#values)

##### animation-timeline

_Experimental_ (Only Chrome supports)

| Value    | Description                                                 |
| -------- | ----------------------------------------------------------- |
| `auto`   | Default; By passing of time since document was first loaded |
| `view()` | By visibility of an element inside the nearest scroller     |

And
[more...](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-timeline#values)

#### Using multiple animations

You can apply multiple animations on a single element.

```
animation:
  typing 3.5s steps(40, end),
  blink-caret .75s step-end infinite;
```

#### Examples

##### Typewriter animations

In this example the animation is as following ("|" is the blinking caret):

1. | Project
2. **My early** | Project
3. | Project
4. **My very armature** | project
5. | Project
6. (start all over again)

```html
<h2>Project</h2>
```

```css
h2::before {
  content: ""; /* need to declare here for animation to change it */
  display: inline-block; /* for width property to take effect */
  white-space: nowrap; /* don't wrap text */
  overflow-x: hidden; /* don't show text beyond the width */
  animation:
    projects-typewriter 6s infinite both linear,
    blinking-caret 0.5s infinite;
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

### Cursor

`cursor` attribute controls cursor style hovering on the element. Actual cursor
icons are dependent on platform and user's computer setting.

#### General cursors

| Value     | Description                             |
| --------- | --------------------------------------- |
| `auto`    | **Default**, UA determine automatically |
| `default` | Platform-dependent default cursor       |
| `none`    | No cursor is rendered                   |

#### Links and status cursors

| Value          | Description                                   |
| -------------- | --------------------------------------------- |
| `context-menu` | Context menu available                        |
| `help`         | Help info available                           |
| `pointer`      | Index finger to indicate a **link**           |
| `progress`     | Program is busy in bg, but can still interact |
| `wait`         | Program is busy, cannot interact              |

#### Selection cursors

| Value           | Description                                  |
| --------------- | -------------------------------------------- |
| `cell`          | Like Excel                                   |
| `crosshair`     | Often used to indicate selection in a bitmap |
| `text`          | Text can be selected                         |
| `vertical-text` | Vertical text can be selected                |

#### Drag and drop cursors

| Value         | Description                |
| ------------- | -------------------------- |
| `alias`       | Creating alias or shortcut |
| `copy`        | Copying something          |
| `move`        | Moving something           |
| `no-drop`     | Cannot be dropped here     |
| `not-allowed` |                            |
| `grab`        | Something can be grabbed   |
| `grabbing`    | Grabbing something         |

#### Resize and scroll cursors

For resizing and scrolling (arrows), the value is
`<direction>[direction]-resize`, where direction is given by `n`, `e`, `s`, `w`,
`ne`, etc.

#### Zoom cursors

| Value      | Description |
| ---------- | ----------- |
| `zoom-in`  | Zoom in     |
| `zoom-out` | Zoom out    |

### Caret

- `caret-color`: colour of the caret
- `caret-shape`: `auto` or `bar` (default), `block`, `underscore`

### Html tables

- `padding` of `<td>` controls distance between content and border of the cell.
- `border-spacing` of `<table>` controls space between border of the cells.
  - Make sure `border-collapse: separate` but not `border-collapse: collapse`
    for `border-spacing` to work on `<table>`
  - In tailwind, it seems the default changes to `collapse`

To make rounded border, say for each row, you must do something like this:

```css
table {
  border-collapse: separate;
  border-spacing: 0 auto; // spacing in x-axis is 0
}

table :is(th, td) {
  border-block: 1px;
}

table :is(th, td):first-child {
  border-left: 1px;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
}

table :is(th, td):last-child {
  border-right: 1px;
  border-top-right-radius: 5px;
  border-bottom-right-radius: 5px;
}
```

### Shadows

- `box-shadow`: h-offset, v-offset, blur, spread, colour
- `text-shadow`: h-offset, v-offset, blur, colour
- `filter: drop-shadow()`

```css
/* Shadow only appears outside the border box. */
box-shadow: 0px 0px 1rem purple;
/* Can see throguh invisible part of the element to the shadow */
filter: drop-shadow(0px 0px 1rem red);
```

### Logical properties

`block`, `inline`, `start` and `end` is according to the writing direction.

- `margin-block` and `margin-inline`
- `padding-inline-start` and `padding-inline-end`
- `max-inline-size` and `max-block-size`

### Themes

- `color-theme`: `light` or `dark` let the browser to render light or dark theme
  of build in elements, such as form elements.
- Media query `prefers-color-scheme`: `light` or `dark`
-

## Queries

### Query conditions

#### Logical keywords

- `and`
- `or`
- `not`: only one `not` is allowed and cannot be used with `and` or `or`

#### Condition

- `(max-width: 500px)`
- `(min-height: 200px)`
- `(width >= 1080px)`
- `(min-width: 640px) and (max-width: 1280px)`

### Container queries

`@container` queries can apply CSS styling based on ancestor element's size.

#### Quick example and syntax

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

#### container

`container: <name> / <type>` is a short hand attribute.

#### container-type

Parent has set `container-type` to be either `inline-size` or `size`

- `inline-size`: query based on **inline** dimension
- `size`: based on both **inline** and **block** dimension

Then you can use container query:

#### container-name

You can also give the container a case-sensitive name using `container-name`
which can be used as a filters in later container queries:
`@container my-container (width > 500px)`.

#### Container conditions

- `width`
- `height`
- `aspect-ratio`
- `block-size`
- `inline-size`
- `orientation`: `landscape` or `portrait`

## BEM Convention

### BEM

**_BEM_** stands for **_Block_**, **_Element_**, **_Modifier_**. It is a
methodology to structure codes in a component approach. It applies to CSS
classes naming conventions and file structure.

| Term     | Description                                                |
| -------- | ---------------------------------------------------------- |
| Block    | Functionally independent page component that can be reused |
| Element  | A composite part of a block that can't be used separately  |
| Modifier | Change appearance, state or behaviour of block or element  |

#### Block

- Can be nested in each other
- Block name describes its purpose, not its state
- Each BEM name has only one block name
  - Block name acts as the namespace

#### Element

- Element name describes its purpose, not its state
- An element name is separated from the block name with a double underscore `__`
- Three general rules: **nesting**, **membership** and **optionality**

##### Nesting rule

- Elements can be nested in each other
- But an element is always part of a block, not another element
  - So `block__elem1__elem2` is not a valid name
  - This allows you to change a block's structure without changing its elements
    name

##### Membership

An element is always part of a block, so you shouldn't use it separately from
the block

##### Optionality

An element is an optional block component. Not all blocks have elements.

#### Block vs Element

Create a block:

- If a section of code might be reused
- It doesn't depend on other page components being implemented

Create an element:

- If a section of code can't be used separately without the parent entity (the
  block)
- The exception is elements that must be divided into smaller parts -
  subelements - in order to simplify development.
  - Since in BEM methodology, you can't create elements of elements. So in a
    case like this, instead of creating an element, you need to create a
    _service block_: a block that also depends on other blocks.

#### Modifier

- Defines the appearance, state, or behaviour of a block or element.
- A modifier name is separated from the block or element name by a single
  underscore `_`.

##### Modifier types

Boolean modifiers:

- If a boolean modifier is present, its value is assumed to be `true`.
- E.g. `disabled`

Key-value modifiers:

- E.g. `menu_theme_islands`

##### Guidelines using modifiers

- Modifier can't be used alone
- Modifier should change, not replace the appearance, behaviour, or state

#### Mix

A technique for using different BEM entities on a single DOM node.

- Combine the behaviour and styles of multiple entities without duplicating code
- Create semantically new UI components based on existing ones

```html
<div class="header">
  <div class="search-form header__search-form"></div>
</div>
```

E.g. `search-form` doesn't specify any specific geometry like padding and
margin, so it is an independent block element. `header__search-form` element
specifies the geometry of the search form in the header.

#### File structure

It is only recommended.

- A single block corresponds to a single directory.
- The block and its directory have the same name.
- A block's implementation is divided into separate technology files, e.g.
  `header.css` and `header.js`.
- The block directory is the root directory for the subdirectories of its
  elements and modifiers.
- Names of element directories begin with a double underscore `__`, e.g.
  `header/__logo`.
- Names of modifier directories begin with a single underscore `_`, e.g.
  `header/_fixed/`, `menu/_theme_islands`.
- Implementations of elements and modifiers are divided into separate technology
  files. E.g. `header__input.js` and `header_theme_islands.css`.

```
search-form/
  __input/
    search-form__input.css
    search-form__input.js
  __button/
    search-form__button.css
    search-form__button.js
  search-form.css
  search-form.js
```

### Naming convention

#### Classic style

`block-name__elem-name_mod-name_mod-val`

- Names are written in lowercase Latin letters.
- Words are separated by a hyphen `-`.
- The block name defines the namespace for its elements and modifiers.
- The element name is separated from the block name by a double underscore `__`.
- The modifier name is separated from the block or element name by a single
  underscore `_`.
- The modifier value is separated from the modifier name by a single underscore
  `_`.
- For boolean modifiers, the value is not included in the name.

Examples:

```html
<div class="menu menu_hidden">
  <span class="menu__item"></span>
  <span class="menu__item menu__item_type_radio"></span>
</div>
```

#### Two dashes style

`block-name__elem-name--mod-name--mod-val`

#### camelCase style

`blcokName-elemName_modName_modVal`

#### React style

`BlockName-ElemName_modName_modVal`

### Reference

- [bem.info](https://en.bem.info/methodology/naming-convention/)

## Tips

### Access value of attribute

```css
/* hover tooltip showing content of aria-label attribute */
button:hover::after {
  content: attr(aria-label);
  position: absolute;
  top: 0;
  left: calc(100% + 0.5em);
  padding: 0.5rem 1rem;
  border-radius: 0.5rem;
  background-color: var(--bg-o85);
  color: var(--fg);
}
```

### Apply a filter

### Translate position

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

### One-liners

#### Width

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

### CSS Grids Layouts

#### Header and footer

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

#### Stacking content

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

#### Stretching out children

Children of a grid would automatically have their size stretched out to fill the
cell. So instead of setting up each different type of elements with
`width: 100%`, you can just do `display: grid`.

### Quotes pseudo-element

```css
::bofre {
  content: open-quote;
}
```

The next open-quote becomes a single quote. To tell CSS not to nest quotes, you
need to close quotes at the end (e.g. at `::after`) with `close-quote` or
`no-close-quote`.

### Isolation

```css
.isolation {
  isolation: isolate;
}
```

Creates a isolated stacking context for `z-index` so children's z-index won't
affect their relative z position to elements outside the parent.

### Inset

```css
inset: auto 1px 2rem 3em;
/* is identical to */
top: auto;
right: 1px;
bottom: 2rem;
left: 3em;
```

### Counters

```css
section {
  counter-increment: my-section-counter;
}
.section-title::before {
  content: counter(my-section-counter) ". ";
  opacity: 0.5;
}
```

### CSS Contain

`contain` property makes certain aspect of CSS styling contained within an
element (no effect outside). Some values:

- `style` contains the `counter-increment` and apply scope to the counter
  variable
- `paint` / `layout` contains layout recalculations, so that, say, animation
  within won't cause outside elements to readjust.

#### Example: style

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

### Filters and focus-within

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

### Invert colour

```css
@media (perfers-color-scheme: dark) {
  .icon {
    filter: invert(100%);
  }
}
```

## Weird Behaviour

### Overflow

If you use `visible` for either `overflow-x` and `overflow-y`, but something
else for the other, the `visible` value is interpreted as `auto`.

### Scroll bar conflicts with justify or place

If you use `auto` or `scroll`, but also use `justify-content: flex-end`,
`place-items: end`, `jusitfy-self: flex-end` or `place-self: end`, then the
scroll bar will be disabled.

You might want to do this, for example, in a terminal output display box or a
chatbox, when you want to justify content at the bottom when the output is too
little and there isn't overflow yet.

To work around, set the first child in the container to `margin-top: auto` to
achieve the same effect without breaking scroll bars when overflow happens.

### Inline-flex and vertical-align

See this
[stack overflow question](https://stackoverflow.com/questions/48117071/element-with-display-inline-flex-has-a-strange-top-margin).

Basically, setting `inline-flex` makes an element an inline-level element, which
activates `vertical-align` property, whose default value is `baseline`, so that
the element is pushed down to align with the baseline of the text. There are two
solutions:

- Override the default value of `vertical-align` to any other value.
- Set parent element to `display: flex`.

### Parent Container Size Bigger Than Content

Could be a side effect of `min-`, `max-` `width` or `height` properties. Try
removing them to see if the problem got fixed.

## 🧭 Navigation

- [📑 Notes Index](../../index.md)
