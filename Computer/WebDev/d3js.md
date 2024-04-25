---
date: 2023-09-17 (Sun)
---

# D3.js

## Data Visualisation

### Two general purposes

1. **Exploratory**
   - Present in unbiased and unleading way
   - Allows audience to connect freely and explore for any insight
   - Richard Feynman: I think on paper. External visual representation augment
     cognition process with perception
   - It helps you to ask new questions you didn't come up with
2. **Explanatory**:
   - Conveys your insights about the data

### 5 key lessons for successful explanatory visualisation

1. Understanding **context**
   - Who the audience is? What they need to know and do?
2. Choosing right **type** of visualisation
3. Removing **clutter**
   - Removing unnecessary visuals to decrease cognitive flow and make data stand
     out
4. Guiding **attentions**
   - Use colours, shapes, sizes and placements
5. Telling a **story**

### Tool Landscape for data visualisation

From low level to high level tools:

1. HTML, CSS, SVG, JavaScript, DOM APIs
2. D3, React, Angular, Panda
3. Vega, VX, Matplotlib
4. Semiotic, RAWGraphs
5. Vega-Lite, Seaborn
6. Altair
7. Vega-lite-api
8. ggplot2
9. Tableau, PowerBI, QlickSense, Tibco Spotfire, Plotly

- Vega allows you declaratively specify visualisation using only a JSON
  configuration object
- Vega-Lite provides default config for Vega
- Vega-lite-api is a JS API to create Vega-Lite specification
- Altair is like Vega-lite-api but in Python
- Seaborn is a Python library
- ggplot2 is reverse engineering of The Grammar of Graphics. Used by many R
  developers to create visualisation in short period of time
  - It inspires Vega, which leads to Vega-Lite, and in turn to Vega-lite-api

### Type of Visualisation

- [Slope graph](https://www.storytellingwithdata.com/blog/2014/03/more-on-slopegraphs)
  - [Alberto Cairo - iterative approach to arrive at slope graph](https://www.vizwiz.com/2013/01/alberto-cairo-three-steps-to-become.html)
- Steam graph: solve the problem of stacked area.

### Marks and Channels

In data visualisation, a **_mark_** of a plot represents an **item** (one datum
point), and **_channels_** are visual modifiers of the mark that signifies its
values of various **fields**.

Examples of marks are point, line, area. Examples of channels are position,
colour (hsla), size and shape.

For hue, a full spectrum is not good to represent fields that are not
categorical, except for multi-hue colourgrams, but they work because they rely
on luminance or saturation, but hue only assist to bring out the contrast.

#### Ranking of channels

_Magnitude_ channels:

- Position on common scale
- Position on unaligned scale
- Length (1D size)
- Tilt / angle
- Area (2D size)
- Depth (3D position)
- Colour luminance and saturation
- Curvature and volume

_Identity_ channels:

- Spatial region
- Colour hue
- Motion
- Shape

## Data

### Formats

- **_Comma Separated Values_** (**_CSV_**)
- **_JavaScript Object Notation_** (**_JSON_**)

### Obtaining data

#### From HTML tables on webpages

1. Select the data (cursor highlight)
2. Copy (Ctrl+C)
3. Paste to a spreadsheet program (e.g. Google Spreadsheet)
4. Modification (e.g. remove columns that can't translate well to CSV)
5. Export to CSV

### Hosting data online

[GitHub Gist](https://gist.github.com/) allows you to upload CSV files and host
it. Click the `Raw` button and you get the URL to access the raw CSV.

- You can shorten the Raw URL by removing the commit version part.

Then you can use `fetch()` API to fetch the data.

It is important to list the _provenance_ (source) of the data.

### Handling data

#### CSV data

##### CSV URL

d3 has `csv()` that takes the URL linking to a CSV file, and returns a
**promise** that resolves into parsed data.

```js
const data = d3.csv(url);
const numOfData = data.length;
const numOfFields = data.columns.length;
```

##### CSV string

d3 has `csvParse()` that takes a CSV string and returns an array of data rows,
with a property `columns` whose value is an array of **header** columns.

```js
const csvSize = csvStr.length; // in bytes
const data = d3.csvParse(csvStr);
const numOfEntries = data.length;
const numOfFields = data.columns.length;
```

##### Format parsed data to CSV string

d3 has `csvFormat()` that takes parsed CSV data returns a CSV string.

#### JSON data

`json(url)` returns a **promise** that resolves into parsed json **object**.

Otherwise, use native JavaScript fetch json methods.

### Types of data

#### Reference

Watch
[Inputs for Visualization: Data & Tasks](https://youtu.be/2LhoCfjm8R4?si=xhi35r_B4qXYD5U1&t=11320).

#### Types of dataset

- Tables
  - Spreadsheet with Attributes (columns) and items (rows)
  - Multidimensional tables
- Networks with nodes and links
  - Trees
- Spatial data
  - Fields
  - Geometry
  - Vector table

#### Attribute types

- Categorical (or nominal)
- Ordered
  - Ordinal
    - Aggregation of quantitative attributes into intervals
  - Quantitative (both non-continuous and continuous)

Some special case:

- temporal data (point in time vs region in time)
- spatial data (point in space vs region in space)

## SVG Path Generator

### Arc

`arc()` returns a function, which returns a string for the attribute `d` of svg
`<path>`.

```js
const mouthArc = arc()
  .innerRadius(mouthRadius)
  .outerRadius(mouthRadius + mouthWidth)
  .startAngle(Math.PI * 0.7) // radian clockwise from North
  .endAngle(Math.PI * 1.3);
```

## Helper Functions

### Range

`range(5)` returns `[0, 1, 2, 3, 4]`.

### Max

`max(arr, projFunc)` returns the max value in an array using natural order and a
projection function to map values to easily-sorted values.

E.g. `max(countriesData, (d) => d.population)` returns the maximum population in
the dataset.

### Formatters

#### Numbers

Use `d3.format()`. See [d3-format](https://github.com/d3/d3-format#d3-format).

Quick examples:

```js
d3.format(".0%")(0.123); // rounded percentage, "12%"
d3.format("($.2f")(-3.5); // localized fixed-point currency, "(£3.50)"
d3.format("+20")(42); // space-filled and signed, "                 +42"
d3.format(".^20")(42); // dot-filled and centered, ".........42........."
d3.format(".2s")(42e6); // SI-prefix with two significant digits, "42M"
d3.format("#x")(48879); // prefixed lowercase hexadecimal, "0xbeef"
d3.format(",.2r")(4223); // grouped thousands with two significant digits, "4,200"
```

##### Number format testing tool

See [Tools](#tools).

#### Dates

##### D3 time format documentation

- [d3-time-format](https://github.com/d3/d3-time-format#d3-time-format)

##### Date object to formatted string

`timeFormat()` accepts a format string, and returns a function, which accepts a
`Date` object and returns a formatted string.

```js
const formatTime = d3.timeFormat("%B %d, %Y");
formatTime(new Date()); // "June 30, 2015"
```

##### Formatted string to date object

`timeParse()` accepts a format string, and returns a function, which accepts a
formatted string and returns a `Date` object.

```js
const parseTime = d3.timeParse("%B %d, %Y");
parseTime("June 30, 2015"); // Tue Jun 30 2015 00:00:00 GMT-0700 (PDT)
```

##### Format string placeholder

- `%a` - abbreviated weekday name.\*
- `%A` - full weekday name.\*
- `%b` - abbreviated month name.\*
- `%B` - full month name.\*
- `%c` - the locale’s date and time, such as `%x, %X`.\*
- `%d` - zero-padded day of the month as a decimal number \[01,31\].
- `%e` - space-padded day of the month as a decimal number \[ 1,31\]; equivalent
  to `%\d`.
- `%f` - microseconds as a decimal number \[000000, 999999\].
- `%g` - ISO 8601 week-based year without century as a decimal number \[00,99\].
- `%G` - ISO 8601 week-based year with century as a decimal number.
- `%H` - hour (24-hour clock) as a decimal number \[00,23\].
- `%I` - hour (12-hour clock) as a decimal number \[01,12\].
- `%j` - day of the year as a decimal number \[001,366\].
- `%m` - month as a decimal number \[01,12\].
- `%M` - minute as a decimal number \[00,59\].
- `%L` - milliseconds as a decimal number \[000, 999\].
- `%p` - either AM or PM.\*
- `%q` - quarter of the year as a decimal number \[1,4\].
- `%Q` - milliseconds since UNIX epoch.
- `%s` - seconds since UNIX epoch.
- `%S` - second as a decimal number \[00,61\].
- `%u` - Monday-based (ISO 8601) weekday as a decimal number \[1,7\].
- `%U` - Sunday-based week of the year as a decimal number \[00,53\].
- `%V` - ISO 8601 week of the year as a decimal number \[01, 53\].
- `%w` - Sunday-based weekday as a decimal number \[0,6\].
- `%W` - Monday-based week of the year as a decimal number \[00,53\].
- `%x` - the locale’s date, such as `%-m/%-d/%Y`.\*
- `%X` - the locale’s time, such as `%-I:%M:%S %p`.\*
- `%y` - year without century as a decimal number \[00,99\].
- `%Y` - year with century as a decimal number, such as `1999`.
- `%Z` - time zone offset, such as `-0700`, `-07:00`, `-07`, or `Z`.
- `%%` - a literal percent sign (`%`).

Directives marked with an asterisk (\*) may be affected by the locale
definition. For more details about `%U`, `%V`, `%g`, `%G`, see the
[doc](https://github.com/d3/d3-time-format#locale_format).

##### Specifying padding

The `%` sign indicating a directive may be immediately followed by a padding
modifier:

- `0`: zero-padding (default except for `%e`)
- `_`: space-padding (default for `%e`)
- `-`: disable padding

##### Time format testing tool

See [Tools](#tools).

## Plotting

### Bar chart

The idea is to map each data item to a `<rect>` in `<svg>`. `<rect>` has four
attributes, `x`, `y`, `width` and `height`.

Note that in a `<svg>` the origin starts at top left, and x-coordinate grows to
the right while y-coordinate grows downwards.

To make it simple, we consider a horizontal bar chart (each bar grows from left
to right). We know that `x` is always 0 for all bars. To calculate `y`, `width`
and `height` we can use a [**scale**](#what-is-a-scale). This scale is
[generated](#generating-a-scale) by a function provided by _d3_.

Let's say along the y-axis (the base of each bar) we want to represent an
categorical or ordinal field, e.g. country. We can use
[`scaleBand()`](https://github.com/d3/d3-scale#band-scales) from _d3_ to
generate a even scale for each field value:

```js
const yScale = scaleBand()
  .domain(data.map((d) => d.country))
  .range([0, height]);
```

Then we can assign `y` attribute of `<rect>` to be `yScale(d.country)`. The
resulting `yScale` also provides the bandwidth which represents the width of
each bar, so we can set the attribute `height` (height because it's a horizontal
bar chart) to `yScale.bandwidth()`.

Along the x-axis, we may want to represent a quantitative data such as
population. We can use
[`scaleLinear()`](https://github.com/d3/d3-scale#linear-scales) to generate a
linear scale that:

```js
const xScale = scaleLinear()
  // domain goes from 0 to max population in the dataset
  .domain([0, max(data, (d) => +d.population)]);
  .range([0, width])
```

- `max()` is a helper function from _d3_.
- The `+` before `d.population` is to type cast a string to a number

Then we can set the `width` attribute to `xScale(+d.population)`.

For other orientation of bar charts, you can use `<g>` to group the bars and use
`transform` attributes to reflect, translate and scale.

### Scatter plot

See [Bar chart](#bar-chart) for general plotting technique.

For scatter plot, the marks are usually `<circle>`. But you can use custom
shapes or any custom SVG element.

### Line graph

Can use [`line()`](#line-generator).

### Map graph

#### Map data and resources

##### Map data file formats

[**_TopoJSON_**](https://github.com/topojson/topojson#topojson) is a file format
extended from [**_GeoJSON_**](https://en.wikipedia.org/wiki/GeoJSON) that
encodes topology.

TopoJSON is better than GeoJSON in that the locus of overlapping lines (e.g.
from country boundaries) are only represented once. But GeoJSON's data is easier
to work with, so you usually want to convert TopoJSON format back to GeoJSON
after fetching the data.

A parsed TopoJSON object is a **_topology_** object. A topology's `type` must be
`"Topology"` and may contain any number of named **_geometry_** objects, which
can be accessed via the `objects` field. A topology may have a `bbox` field
whose value is a bounding box array. A geometry object's `type` is
`"GeometryCollection"`. See
[TopoJSON specification](https://github.com/topojson/topojson-specification#2-topojson-objects)
for more details.

A GeoJSON object contains both geographical **_features_** and their non-spatial
attributes. The features include spatial info comprised of points (represented
by latitude and longitude). It's `type` must be `"FeatureCollection"` and it has
an array of **_feature_** object via the field `features`.

A feature's `type` is `"Feature"`. It's `geometry` is the spatial info and
`properties` is the non-spatial info in key-value pairs.

###### TopoJSON to GeoJSON

[topojson-client](https://www.npmjs.com/package/topojson-client) npm package
provides tools to manipulate TopoJSON and covert it to GeoJSON for rendering
with standard tools such as [`d3.geoPath`](#d3-geo).

```js
import { feature } from "topojson-client";
const geojson = feature(topojson, topojson.objects.countries);
```

###### GeoJSON to TopoJSON

[topojson-server](https://www.npmjs.com/package/topojson-server) npm package
provides tools to convert GeoJSON to TopoJSON.

##### World map

[topojson/world-atlas](https://github.com/topojson/world-atlas) provides the
_TopoJSON_ derived from the _Natural Earth_ project.

[_Natural Earth_](https://www.naturalearthdata.com/) is a public domain map
dataset, built through a collaboration of many volunteers. It's available at
**different levels of details** suitable for different zoom levels. _Cultural
vectors_ dataset provides suitable level of details for countries. This is
exposed to us by _topojson/world-atlas_. _topojson/world-atlas_ also provides
multiple level of details, and you download or use the URL of the TopoJSON of
your desired details.

##### Reducing size of data file

###### Online tool

[Mapshaper](https://mapshaper.org) helps you visualise geographical shapes. It
also helps you convert among Shapefile, GeoJSON, TopoJSON, DBF and CSV. Some map
data files are quite big with too many points, e.g. Shapefiles usually are. This
[website](https://bost.ocks.org/mike/simplify/) explains _Visvalingam's
algorithm_ that can remove least impactful points. _Mapshaper_ has this feature
to reduce the size of the output file.

You can then host the data, e.g. on GitHub gist.

###### npm package

[topojson-simplify](https://www.npmjs.com/package/topojson-simplify) npm package
provides tools to simplify and do filtering for TopoJSON, to reduce the size and
make rendering faster.

#### Visualising map data

TopoJSON uses longitude and latitudes. We need to do a projection to transform
them into 2D coordinate.

##### d3-geo

###### Docs

See [d3-geo](https://github.com/d3/d3-geo#d3-geo) docs.

###### geoPath generator

We use `geoPath([projection])` to create a geo-path data string generator. We
need to supply the path generator with a
[projection](https://github.com/d3/d3-geo#projections), then can we invoke it
with a feature array from GeoJSON as an argument to generate the data string.

```js
const projection = geoEqualEarth();
const geoPathGenerator = geoPath(projection);
return (
  <>
    {geojson.map((feature) => (
      <path d={geoPathGenerator(feature)} />
    ))}
  </>
);
```

A geoPath generator can also invoked with `{type: "Sphere"}` as an argument to
output a data string that visualise the sphere of the projection, i.e. the
background of the map.

###### Instance methods

- `projection([projection])` to provides a
  [projection](https://github.com/d3/d3-geo#projections).
- `bounds([geojson])` returns the projected planar bounding box for the set or
  specified GeoJSON object. The bounding box is a two-dimensional array:
  `[[minX, minY], [maxX, maxY]]`. Note that the minimum latitude is typically
  the maximum y-value and vice versa.
  - It seems the natural viewBox is more or less 960 x 500 most of the time,
    where the viewBox also includes the sphere projection.
  - Not to be confused with `geoBounds([geojson])`. Which returns the longitude
    and latitude bounding box for sphere projection.

##### Manipulating map data

- `topojson.mesh(topology, object, filterFunc)` prunes overlapping lines

##### Plot latitudes and longitudes

- `d3.geoGraticule()`

#### Example

```js
import { feature } from "topojson-client";
import { geoPath, geoEqualEarth } from "d3";

const projection = geoEqualEarth();
const geoPathGenerator = geoPath(projection);

export default function Map({ topojson }) {
  const geojson = feature(topojson, topojson.objects.countries);
  const [[x0, y0], [x1, y1]] = geoPathGenerator.bounds(geojson);

  return (
    <svg viewBox={`${x0} ${y0} ${x1} ${y1}`}>
      {geojson.features.map((feature, i) => (
        <path key={i} d={geoPathGenerator(feature)} />
      ))}
    </svg>
  );
}
```

## Shapes Generators

### D3 shapes docs

- [d3-shape](https://github.com/d3/d3-shape#d3-shape)

### Shape generators

A **_shape generator_** is a path data string generator that accepts an array of
data, and returns a SVG path data string. It can be used in
`<path d={pathStr} />`.

D3 provides functions to create a shape generator. And you can chain methods to
a created shape generator to customise the shape before calling it with data as
an argument.

For a 2D shape, minimally, you need to set up the x-coordinate accessor and
y-coordinate accessor before calling it:

- `x(x)`: sets up the x-coordinate accessor, `x( xScale( xVal(d) ) )`
- `y(y)`: sets up the y-coordinate accessor.

### Line Generator

`line()` creates a line generator. It is a [shape generator](#shape-generators)
Instance methods of line generator:

- `curve([curve])`, sets the
  [curve factory](https://github.com/d3/d3-shape#curves). Default is
  `curveLinear()`. See
  [Observable: d3.line](https://observablehq.com/@d3/d3-line) has some example
  of different curves, and you can select and observe their behaviour.

#### Curve factory

- `curveLinear()`: linear line from each pair of data points
- `curveNatural()`: smooth curve
- `curveBasis()`: very smooth, but line doesn't always hit the data points
- `curveCardinal().tension()` control how curve between data points.

## Scales

### What is a scale

A **_scale_** is a function that takes an input (here it is the value of a field
of an data item) and returns a corresponding coordinate that represent this
input within a range. In short, a mapping from a domain to a range. Once we have
a scale, we can simply pass a field value of each data item to it and obtain the
position within the range that represents that field value.

### Generating a scale

#### Defining domain and range

To use scale-generating functions in _d3_, you need to specify the domain and
range of the scale. Different scale-generating functions generate different
kinds of scales, like linear scale, log scale and etc. The code structure is
generally:

```js
const scale = scaleGeneratingFunc().domain(domain).range(range);
```

#### Customising a scale

You can chain further functions to customise the scale.

- `nice()`: extends the domain so it ends on nice round numbers.
  - Optionally accepts a tick count to control the step size used to extend the
    bounds.

### Scale instance properties

### Overview of scale types

| Type of Scale | Scale generating function |
| ------------- | ------------------------- |
| Ordinal scale | `scaleBand()`             |
| Linear scale  | `scaleLinear()`           |
| Time scale    | `scaleTime()`             |

- `scaleTime()` can accept JavaScript `Date` objects.

## Margins and Axes

### The Margin Convention

See diagram from
[YouTube: Margins and Axes](https://youtu.be/2LhoCfjm8R4?si=qgNn6Z0MZZQfK57N&t=17916).

The origin is the top left corner of the `<svg>`. The inner space for marks are
wrapped in `<g>` and translated by left and top margin. This inner space has its
own `innerWidth` and `innerHeight`.

We define `const margin = {top: 20, right: 20, bottom: 20, left: 20}` and
calculate `innerWidth` and `innerHeight` correspondingly.

### Ticks of axes

For a generated continuous `scale`, you can use `scale.ticks()` to get a list of
position for ticks. See
[ticks of a continuous scale](https://github.com/d3/d3-scale#continuous_ticks).
It returns approximately _count_ representative values from the scale's domain.
If _count_ is not specified, it defaults to 10.

For a generated non-continuous scale, you can use `scale.domain()`. This returns
what we passed to the domain earlier.

For ticks, you use `<line>` and for labels you use `<text>`. You can put these
elements within the `<g>` for marks space and then translate it to the desired
position.

```js
{
  xScale.ticks().map((tickVal, i) => (
    <g key={tickVal} transform={`translate(${xScale(tickVal)}, 0)`}>
      <line y2={innerHeight} stroke="black" />
      <text y={innerHeight} dy="1em" style={{ textAnchor: "middle" }}>
        {`${(tickVal * 1e-6).toFixed(1)}B`}
      </text>
    </g>
  ));
}
{
  yScale.domain().map((country, i) => (
    <g key={country} transform={`translate(0, ${yScale(country)})`}>
      <text
        dx="-1em"
        dy={yScale.bandwidth() * 0.5}
        style={{ textAnchor: "end" }}
      >
        {shortenCountryName(country)}
      </text>
    </g>
  ));
}
```

### Formatting ticks

See [Formatters](#formatters) for helper functions that can help you format the
values.

## Tooltips

For a very simple way to make tooltips, we can use
[HTML SVG - Hover Tooltip](html-svg.md#hover-tooltip).

## Key Concepts

### Accessors

An accessor is a function that takes in a object and return the value of a
particular field. In the context of handling data, it takes in a data item and
return the value of a particular field.

For instance, with an accessor, instead of passing `d.population` and
`(d) => d.population` (e.g. for `map()`) in multiple places, you can just define
an accesor `const popAcsr = (d) => d.population`, and pass `popAcsr(d)` and
`popAcsr` to sub-components (dependency injection).

In D3, you will need to provide or receive accessors for various functions.

## Notes on Other Tools

### Vega Lite API

#### Terms for Data

- A data frame refer to a table dataset
- A data field refers to a column in the table dataset

#### Functions

- `printTable()` takes a data frame and prints a table representing it

## Links and Resources

### Course Link

- The course [video](https://www.youtube.com/watch?v=2LhoCfjm8R4) on YouTube
- Sunlight Foundation Data Visualization Style Guide

### Import Node Modules through CDN

[unpkg.com](https://unpkg.com/)

- An URL scheme to load any file from any package:
  `<script src="unpkg.com/react@16.7.0/umd/react.production.min.js></script>"`

### Modules bundler

Bundle modules into one file. There's a plugin that transpires JSX into JS in
the bundler [rollup.js](https://rollupjs.org/). The result is a bundled JS file
`bundle.js`. Then you source this script with
`<script src="bundle.js"></script>`.

### Courses

- [Data Visualization and D3.js - Udacity](https://learn.udacity.com/courses/ud507)

### Examples

- [NYT Graphics @ Twitter](https://twitter.com/nytgraphics)
- [Reuters Graphics](https://www.reuters.com/graphics/)
- [NYT The Upshot](https://www.nytimes.com/international/section/upshot)
- [The Pudding](https://pudding.cool/): Really cool visual essays
- [FiveThirtyEight Data Visualization](https://fivethirtyeight.com/tag/data-visualization/)
- [Reddit r/dataisbeautiful](https://www.reddit.com/r/dataisbeautiful/)
- [Information is Beautiful Award](https://www.informationisbeautifulawards.com/)
- [Graphic Detail - By The Economist](https://www.economist.com/graphic-detail)
- [Blockbuilder Search](http://blockbuilder.org/): Many Bl.ocks - examples of D3
  with underlying code
  - Sadly it shut down. But you can use Observable below
- [Observable community](https://observablehq.com/community#highlights)
- [Our World in Data](https://ourworldindata.org/)

### Blogs

- [FlowingData](https://flowingdata.com/)
- [Observable community](https://observablehq.com/community#highlights)
- [Zan Armstrong](https://www.zanarmstrong.com/)

### Data source

- [Our World in Data](https://ourworldindata.org/)

#### Datasets to play around

- The Iris Dataset
- [Sense Your City](https://grayarea.org/initiative/data-canvas-sense-your-city/),
  [Sense Your City Live Map](https://grayarea.org/initiative/data-canvas-sense-your-city/)

### Tools

- [Zan's D3 number format tool](https://blocks.roadtolarissa.com/zanarmstrong/raw/05c1e95bf7aa16c4768e/index.html)
- [Zan's D3 date/time format tool](https://blocks.roadtolarissa.com/zanarmstrong/raw/ca0adb7e426c12c06a95/index.html)

### Visualisation resources

- [Map data and resources](#map-data-and-resources)

## 🧭 Navigation

- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
