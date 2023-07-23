---
title: CSS
---

# Basics

## Syntax

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

-   `*`
-   `h1`: html element
-   `#id`: id
-   `.class`: class
-   `h1, h2`: Multiple html element
-   `img.cover-img`: element with a class
-   `article > h3`: an element which is a child of a parent

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

-   E.g. two class selectors add 2 times 10 points in specificity value.

## Box Model 🔲

From center to out:

-   Content
-   Padding
-   Border
-   Margin
    -   I think margin of adjacent elements can overlap. Effectively the bigger
        one takes effect.

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

-   `px`: Pixel
-   `rem`: Root (i.e. the whole document) font size

    -   Default is 16px, so 2rem is 32px under default font-sizs.
    -   `:root { font-size: 18px; }` set a different root font size
        -   `* { }` also includes `:root { }`
        -   `html { }` is `:root { }`

-   `em`: Parent font size
-   `%`: 1% of parent's width, unless it is used for the property `height`.
-   `vw`: 1% of viewport width
-   `vh`: 1% of viewport height

## Calculation

```css
.selector {
    width: calc(10px + 20%);
}
```

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

# Properties

## Background

-   `background-blend-mode`:
    [list of values](https://css-tricks.com/almanac/properties/b/background-blend-mode/)
    -   `multiply`

## scroll-snap-align

Make scrolling snap to certain objects.

You need a parent element containing direct children:

```html
<div class="container">
    <div class="snap-align-item"></div>
    <div class="snap-align-item"></div>
    <div class="snap-align-item"></div>
    <div class="snap-align-item"></div>
</div>
```

```css
div.container {
    scroll-snap-type: [x|y|both] [mandatory|proximity];
    overflow-y: scroll;
}
div.snap-align-item {
    scroll-snap-align: [center|start|end];
}
```

## Positioning

-   `position`:
    -   `relative`:
        -   The position the element originally should have occupied is still
            considered occipied.
        -   The element move relative to it's own original position.
    -   `absolute`:
        -   The position the element originally should have occupied is not
            occupied, no mattered it is moved or not. That is, the element is as
            if **non-existent in HTML flow**.
        -   The element move relative to it's first ancestors that is in
            `relative` position. Furthest is the `<html>` tag.

## Display: flex box ⏹️

`display: flex`: Set the flow to be using **_flex box_** model. The **_main
axis_** is the major direction where flex items are arranged, and the **_cross
axis_** is the secondary direction. Some properties for flex box model:

-   `flex-direction: row|column`: Direction of the main axis
-   `justify-content`: How to justify content on each line along the main axis:
    -   `flex-start`
    -   `flex-end`
    -   `space-between`: Even space between each item
    -   `space-evenly`: Even space between each item and between item and the
        boundary of the parent
    -   `center`
-   `align-items`: How to align items on the same line along the cross axis
    -   `flex-start`
    -   `flex-end`
    -   `center`
    -   `stretch`: All item occupy all space available along the cross axis.
-   `flex-wrap: nowrap|wrap`: Whether the items go to next line when there is
    not enough space. Default is nowrap, and item keep shrinking.
-   `gap: <row-gap> <column-gap>`: Gap between items along rows and columns. If
    only one value, it applies to both rows and columns.

You can control each flex-item individually:

-   `align-self`: Like in `align-items`, but target this item only.

Here is a [website](https://flexboxfroggy.com) that let you practice working
with flex box.

# Tips

## Aspect Ratio

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

# 🧭 Navigation

-   [🔼 Back to top](#)
-   [📑 Notes Index](../index.md)
-   [🗃️ Index](/media/mikeX/Nextcloud/index.md)
