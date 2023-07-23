---
title: jsdoc
---

# ts-check

With jsdoc specifying the types, you can enable type checking in some of the IDE
by adding the line `// @ts-check`.

# Basic syntax

```javascript
/**
 * Double astericks defines a documentation
 */
```

# Function

```javascript
/**
 * Function description
 *
 * @param {string} s - A string param
 * @param {number} [n] - An optional number param
 * @param {number} [n=1] - With default value
 * @returns {string} Return value description
 *
 * @example
 *
 *      foo('hello')
 */
```

-   For `{type}`, see chapter [Type Name](#type-name).
-   For `{@param}`, see chapter [Parameters](#parameters).

# Type Name

-   `{*}`: Any type
-   `{boolean}`, `{myNamespace.MyClass}`
-   `{(number|boolean)}`: Can have one of several type
-   `{Array.<MyClass>}`, `{MyClass[]}`: Array of MyClass instances
-   `{Object.<string, number>}`: String keys and number values
-   `{{a: number, b:string, c}} myObj` or
    ```
    {object} myObj
    {number} myObj a
    {string} myObj.b
    {*} myObj.c
    ```
    or
    ```
    {{
    a: number,
    b: string,
    c
    }} myObj
    ```
-   `{Promise<string[]>}`: Promise fulfilled by array of strings
-   `{?number}`: Number or null
-   `{!number}`: Cannot be null
-   `{myCallback}`: A callback function defined elsewhere with `@callback`

# Parameters

-   `@param {...number} num`: Variable number of numeric param
-   Optional param:
    ```javascript
    /**
    @param {number} [foo]
    @param {number=} foo
    ```
-   `@param {number} [foo=1]`: With default value

# Callback

```javascript
/**
 * Callback description
 * @callback requestCallback
 * @param {number} responseCode
 * @param {string} responseMessage
 */
```

```javascript
/**
 * A callback as part of the requester class.
 * @callback Requester~requestCallback
 * @param {number} responseCode
 * @param {string} responseMessage
 */
```

# Typedef

```javascript
/**
 * A song
 * @typedef {Object} Song
 * @property {string} title - The title
 * @property {string} artist - The artist
 * @property {number} year - The year
 */
```

```javascript
/**
 * A song
 * @typeof {{title: string, artist: string, year: number}} Song
 */
```

# Class

## Syntax

-   `@class [<type> <name>]`

## Prototypes

```javascript
/**
 * Creates a new size.
 * @class {Object<string, number>} Size
 * @param {number} [width=80] Default is 80.
 * @param {number} [height=60] Default is 60.
 */
function Size(width = 80, height = 60) {
    this.width = width;
    this.height = height;
}
```

## Classes

```javascript
/**
 * Create a new program window.
 * @class {object} ProgramWindow
 * {Size} ProgramWindow.screenSize
 * {Size} ProgramWindow.size
 * {Position} ProgramWindow.position
 */
class ProgramWindow {
    constructor() {
        this.screenSize = new Size(800, 600);
        this.size = new Size();
        this.position = new Position();
    }
}
```

# Throw

List possible kinds of error that the function could throw.

```javascript
/**
 * @throw {ArgumentError|OverheatingError|Error}
 */
```

# Links

```javascript
/**
 * {@link namepathOrUrl}
 * [link text]{@link namepathOrUrl}
 * {@link namepathOrUrl|link text}
 * {@link namepathOrUrl link text (after the first space)}
 */
```

# Reference

-   [jsdoc documentation](https://jsdoc.app)
