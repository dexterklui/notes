---
title: bootstrap
---

# Introduction

**_Bootstrap 5_** is a CSS framework that provides

- Beautiful styling
- Styling just by defining classes in HTML elements

# Installation and usage

## Using CDN

**_Content Delivery Network (CDN)_** allows access of resources from nearest
server among many.

Using this method, we don't need to download Bootstrap and import it in our
source code. We only need to include an URL to **link** to the Bootstrap CSS
framework (as well as its own javascript).

Put the style sheet in the heading and before your custom style sheet so that it
can be overridden. Then put the javascript just before the closing `</body>`.

## Direct download

Download Bootstrap CSS and javascript files, and link the downloaded files in
your own HTML file.

# Useful extensions

In VSCode, the extension `HTML CSS Support` provides recommendated/available
bootstrap classes that you can use, using the key bind `<C-i>`.

# Innate styling

## Buttons

They provide serveral predifined button styles, each with its own **semantic
purpose**. You can apply the styles by defining a class:

- btn
- btn-primary
- btn-secondary
- btn-danger

# Layouts

## Flexbox

The class `.d-flex` defines a flex box. You can then use further classes:

- Flex direction: `.flex-column`, `.flex-row`, `.flex-column-reverse`
- Justify content: `.justify-content-center`

## Column system

To use Bootstrap column system, define a `.container` element. It contains
`.row`s, each of which include `.col`s.

By default, each row has **12** equal-width columns. A `col-3` will occupy the
three columns. Unoccupied columns are left blank.

## Grid system

Like column system, define a `.container` element. Then a `.row .row-cols-3`
defines a left-to-right top-to-bottom section where each row has 3 columns.
Inside it, you can define `.col` elements (note that there is no number
following "col").

By default, the columns on a single row automatically fill up available
horizontal space. Defining `.row-cols-auto` let columns flow without
automatically stretch to fill up the available space.

To make the number of columns per row **responsive** to media size, defines
`.row-cols-<media>-4 .row-cols-<media>-2`.

Dunno why, but my testing suggest adding margin would break the layout

# Box models

Paddings (less to more): `.p-0` to `.p-5`. You can also overwrite each of their
padding size with your own CSS code.

- `.ps-0`: padding-start
- `.pe-0`: padding-end
- `.pt-0`: padding-top
- `.pb-0`: bottom
- `.pl-0`: left
- `.pr-0`: right
- `.px-0`: padding along x-axis, i.e. left and right

Margin is like paddings, but use `.m`.

# Media breakpoints

When screen width is larger or equal to certain value:

- X-Small is default
- Small `-sm-`: 576 px
- Medium `-md-`: 768px
- Large `-lg-`: 992px
- Extra large `-xl-`: 1200px
- Extra extra large `-xxl-`: 1400px

Put this before the last number.

# Colour

Colours:

- `-primary`
- `-secondary`
- `-tertiary`
- `-success`
- `-info`
- `-danger`
- `-light`
- `-dark`
- `-link`

Elements:

- `.text`
- `.bg`
- `.btn`

Components:

- `-border-`

# Alignment

`.text-start`, `.text-center`, `.text-end`

# Common Bootstrap Components

- Cards
- Buttons
- Navbar
- Badge

# 🧭 Navigation

- [🔼 Back to top](#)
- 📑 [Index](../../../index.md)
