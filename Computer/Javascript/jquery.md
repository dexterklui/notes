---
title: jQuery
date: 2023-07-23 (Sun)
---

# Introduction to jQuery

## Brief introduction

**_jQuery_** is a JavaScript library designed to simplify HTML DOM tree
traversal and manipulation, as well as event handling, CSS animation and AJAX.

It's free and open-source. Widely supported with good documentation. But it is
becoming superseded by new front-end libraries and frameworks like React.

## Installing jQuery

For **CDN**: Choose a version in [jQuery CDN](https://releases.jquery.com) and
include before `</body>` in the HTML file.

Or download the source JavaScript file from
[jQuery Download Centre](https://jquery.com/download/), and include the local
copy.

## Activating code completion

Put the following config in `jsconfig.json` in the project root to activate
intelligent code completion:

```json
{
  "typeAcquisition": {
    "include": ["jquery"]
  }
}
```

## jQuery vs React

People seldom use jQuery in React. Because React uses a virtual DOM, so using
jQuery (and even native JavaScript) to manipulate the real DOM can be
non-efficient and inconsistent.

## Common jQuery DOM API uses

- Insert elements
- Delete elements
- Change the text
- Change the attribute
- Change the CSS style

# Basic Concepts

## JQuery code

JQuery codes are written as a part of JavaScript code. jQuery exposes its
properties and methods through two properties of the `window` object: `jQuery`
and `$` (which is just an alias for `jQuery`).

## JQuery objects and naming conventions

To provide its own methods and syntactical sugars, jQuery wraps DOM elements
within its own object `JQuery<HTMLElement>`. It is different from `HTMLElement`
(or `NodeListOf<HTMLElement>`), and they do not share methods and properties.

So it is a good practice to prepend variable names for jQuery objects with `$`.

## JQuery methods

Generally two types of methods:

- **_jQuery object methods_**
- **_Core jQuery methods_**

### JQuery object methods

JQuery object methods **_implicitly iterate_** over the whole selection and
**return the same** selection. This is useful for **methods chaining**.

They are in `$.fn` namespace. They automatically receive and return the
selection as `this`. We refer to them by its name: `.each()` or `each()`.

### Core jQuery methods

Core jQuery methods do not work with selections. They are in `$` namespace and
are generally utility-type methods. We refer to them as `$.each()`.

# Waiting until DOM Ready

- Code inside `$( ... )` only executes once DOM is ready
  - Deprecated old syntax: `$(document).ready( ... )`
- Code inside `$(window).on("load", function() { ... })` only executes once the
  entire page (including images and iframes) is ready.

To run multiple statements: `$( function() { ... } )`.

# Element Queries

## Query syntax

Use `$( )`. It works like `querySelectorAll( )`, but returns
`JQuery<HTMLElement>` instead of `HTMLElement[]`.

## CSS selectors

Support _CSS 1-3_. See
[jQuery selectors documentation](http://api.jquery.com/category/selectors/).

- All selector `*`
- Animated selector `:animated`
- Attribute **equals** selector `[name="value"]`
- Attribute **not equals** selector `[name!="value"]`
- Attribute contains **prefix** (followed by `-`) selector `[name=|"value"]`
- Attribute contains word (delimited by spaces) selector `[name~="value"]`
- Attribute starts with selector `[name^="value"]`
- `:button` selects all button elements and elements of type button

## Testing if selection empty

`if ( $(".my-class").length ) { ... }`

## Saving selections

If you need to reuse a selection, it is good to save them in a variable:

```javascript
const $divs = $("div");
```

But if new elements are added, you need to do the query again to include them.

## Methods for queries

All these methods are **_DOM traversal methods_**. All of them accept an
optional **selector** to further filter the result, except `eq()`.

### Filtering selections

| Method             | Description                       |
| ------------------ | --------------------------------- |
| `first()`          | First element (if match)          |
| `last()`           | Last element (if match)           |
| `eq(idx)`          | N-eth element (0-based)           |
| `has(selector)`    | Elements with matching descendant |
| `not(selector)`    | Elements that do not match        |
| `filter(selector)` | Elements that match               |

I guess `not()` is like `:not()` and `filter()` is like `:is()`

### Parents

| Method                   | Description                                 |
| ------------------------ | ------------------------------------------- |
| `parent()`               | Direct parent (if match)                    |
| `parents()`              | All parents                                 |
| `parentsUntil(selector)` | All parents up to but not including a match |
| `closest(selector)`      | Closest match (including self) up the tree  |

### Descendants

- `find(selector)` searches through **descendants** and select matching
  elements.
- `children(selector)` is like `find()` but only traverses one level down.

### Siblings

| Method                | Description                                          |
| --------------------- | ---------------------------------------------------- |
| `prev()`              | Previous element (if match)                          |
| `next()`              | Next element (if match)                              |
| `siblings()`          | All siblings                                         |
| `nextAll()`           | All following siblings                               |
| `nextUntil(selector)` | All following siblings until but not including match |
| `prevAll()`           | All preceding siblings                               |
| `prevUntil(selector)` | All preceding siblings until but not including match |

### Undoing previous filters

All DOM traversal methods pushes a new selection to a stack. `end()` method pop
the current selection from the stack to undo the last filtering.

```javascript
$("#content")
  .find("h3")
  .eq(2)
  .html("new text for the third h3!")
  .end() // Restores the selection to all h3s in #content
  .eq(0)
  .html("new text for the first h3!");
```

### Avoid long traversal

Complex traversal makes it imperative that the document's structure remain the
same, which is difficult to guarantee. Keep it to one or two-step traversal.

# Wrapping and getting native DOM

## Wrapping native DOM

Just wrap it inside `$()`. E.g. `$(event.target)`.

## Getting native DOM elements

Instead of `eq(idx)`, use the following way to get a native DOM element:

1.  `get(idx)`
2.  Bracket notation `[idx]`.

Getting native DOM element is required for making **comparisons**. A new jQuery
object is created every time using `$()` query. Even if two selections have the
same single element, they are still different if they are separate jQuery
instances. To test if two elements are the same, you need to get the native DOM
elements and then do a `===` comparison.

The `get()` without argument gives you back an **array** (even only one element)
of JavaScript objects wrapped inside the jQuery object.

# Creating Elements

## With HTML code

```javascript
$("<p class='text'>A new paragraph</p>");
```

## With attribute lists

```javascript
$("<img/>", { src: "path/to/img", alt: "description" });
$("<div>", { id: "foo", class: "a" });
```

# Modifying and Inspecting Elements

## Getters and setters

- Setters affect **all** elements in a selection.
- Setters return back the selection for method chaining.
- Getters return the **requested value** of the **first** element, except
  `text()`.

## Attributes and CSS

| Method                   | Description                             |
| ------------------------ | --------------------------------------- |
| `css(property)`          | Get value of CSS property               |
| `css(property, value)`   | Set value of CSS property               |
| `css(properties)`        | Set CSS using key-value pairs           |
| `addClass(class)`        | Add new class                           |
| `removeClass(class)`     | Remove class (no error if non-existent) |
| `toggleClass(class)`     | Toggle class                            |
| `attr(attribute)`        | Get value of attribute                  |
| `attr(attribute, value)` | Set value of attribute                  |
| `attr(attributes)`       | Set attribute from key-value pairs      |
| `val()`                  | Get value of `value` attribute          |
| `val(value)`             | Set value of `value` attribute          |

| Methods           | Description       |
| ----------------- | ----------------- |
| `hasClass(class)` | True if has class |

### Using functions to calculate values

For setters, you can pass function references for calculating values in real
time. The function references can be pass directly to the object method, or
place as the value in a key-value pair.

The function will receive two arguments. First is the zero-based index of the
element; second is the current value of the attribute being changed.

## Content and HTML

| Method                         | Description                          |
| ------------------------------ | ------------------------------------ |
| `text()`                       | Get (combied) `textContent`          |
| `text("some text")`            | Set textContent like `textContent()` |
| `html()`                       | Get innerHTML                        |
| `html("<p>a new passage</p>")` | Set innerHTML like `innerHTML()`     |

## Geometry

| Method       | Description                                                |
| ------------ | ---------------------------------------------------------- |
| `width()`    | Get or set the width of the first element                  |
| `height()`   | Get or set the height of the first element                 |
| `position()` | **Get** an object with position info for the first element |

Default unit is "px" unless specified.

# Manipulating DOM

Note that these methods only work on elements queried by **jQuery**, i.e. by
`$()`. If it is a list, then apply to **all** elements inside.

## Inserting elements

- Note that the following methods will **move** away an existing element in a
  DOM.
- **All** following methods can accept **both** already-created elements and new
  elements constructed in place using HTML (e.g. `"<p>text</p>"` or `"<p>"`)

| Method           | Description                        |
| ---------------- | ---------------------------------- |
| `append()`       | Append a child element             |
| `appendTo()`     | Append this element to a parent    |
| `prepend()`      | Prepend a child element            |
| `prependTo()`    | Prepend this element to a parent   |
| `before()`       | Inject HTML before this element    |
| `insertBefore()` | Inject this HTML before an element |
| `after()`        | Inject HTML after this element     |
| `insertAfter()`  | Inject this HTML after an element  |

### Two approaches

1.  Place the selected element(s) relative to another element (e.g.
    `appendTo()`).
2.  Place an element relative to the selected element(s) (e.g. `append()`)

Both approach returns the original selection (that called the method). So you
may want to use one over another depending on what selection you want to pass
along for chaining.

### Costly insertion

Note that though it is convenient to insert elements with jQuery, it has
performance cost to adding to DOM repeatedly. You should **concatenate all HTML
into a single string** and append the string instead if possible.

```javascript
$("#my-container").append(
  `
  <section class="sections" id="update-contact">
    <h1>${title}</h1>
    <form id="${formName}-form" class="contact-form">
      <div id="row-name">
        <input type="text" name="name" class="name" placeholder="Name" />
      </div>
      <div id="row-phone">
        <input
          type="text"
          name=${inputName}
          class=${inputClass}
          placeholder=${inputPlaceholder}
        />
      </div>
      <div id="row-email">
        <input type="text" name="email" class="email" placeholder="Email" />
      </div>
      <div id="row-submit">
        <input type="submit" value="submit" />
      </div>
    </form>
  </section>  
  `
);
```

## Cloning elements

Use `clone()`. Can further chain methods.

## Removing elements

| Method     | Description                                     |
| ---------- | ----------------------------------------------- |
| `remove()` | Remove selected elements from DOM               |
| `detach()` | Like remove but retain attached data and events |
| `empty()`  | Clear innerHTML                                 |

Both `remove()` and `detach()` returns the selection removed

## Replacing elements

| Method                 | Description                   |
| ---------------------- | ----------------------------- |
| `replaceWith(element)` | Replace with given element(s) |

Note that it returns the original jQuery object, and the removed elements will
have their data and event handlers removed.

# Comparing Elements

You need to turn jQuery DOM to native DOM element. See
[Getting native DOM element](#getting-native-dom-element)

# Explicit Iteration

## each()

`each(callback)` calls the callback on each element in the selection. It passes
two arguments to the callback: `function(idx, element)`.

1.  zero-based index indicating the iteration number
2.  current element (`HTMLElement` **without** wrapping in jQuery object)

Also, the callback is called in the context of the current element ( **without**
wrapping in jQuery object), so `this` inside refers to that (**unless** it's an
arrow function).

You can **break** the loop by returning `false`.

## map()

`map(callback)` works like JavaScript's counterpart, except it works on jQuery
object, returns a jQuery object, and passes arguments (**without** wrapping in
jQuery object) to the callback like jQuery's `each()`.

At the end, you may want to chain `get()` to turn the result into native
JavaScript array.

# Utility Methods

They are _Core jQuery methods_ that resides in the `$` namespace. By the way,
some methods may be deprecated (followed by "!"). You should use native
JavaScript code instead because deprecated methods may return incorrect result
when checking against new technology.[^1]

| Method                      | Description                               |
| --------------------------- | ----------------------------------------- |
| `$.trim(string)`            | Remove leading and trailing whitespace    |
| `$.each(iterable, callback` | Call callback(key, val) on iterable       |
| `$.inArray(val, array)`     | Return a value's index, or -1 if none     |
| `$.extend(obj0, obj1...)`   | Change properties of obj0 using objs      |
| `$.extend({}, obj...)`      | Create new obj using objs                 |
| `$.proxy(func, this)` !     | Bind `this` context and return a function |
| `$.proxy(obj, objMethod)` ! | Return a function binding `this` to obj   |

| Method                | Description                      |
| --------------------- | -------------------------------- |
| `$.isArray(arg)`      | Return true if arg is an array   |
| `$.isFunction(arg)` ! | Return true if arg is a function |
| `$.isNumeric(arg)` !  | Return true if arg is a numeric  |
| `$.type(arg)` !       | Return the internal class        |

Unlike `typeof`, `$.type()` doesn't return `object` only, but can be:

- boolean
- number
- string
- function
- array
- null
- regexp
- date

[^1]:
    [GitHub - Deprecate jQuery.type](https://github.com/jquery/jquery/issues/3605)

# Data Methods

In JavaScript, storing data may lead to memory leaks. In jQuery, it manages
memory issues for you.

To get or set data to an element, use `data()` method:

| Method             | Description                     |
| ------------------ | ------------------------------- |
| `data()`           | Receive data in key-value pairs |
| `data(key)`        | Receive data value of a key     |
| `data(key, value)` | Store data                      |
| `data(object)`     | Store data from key-value pairs |

Besides literal data, it can be useful to store reference to other elements so
you don't need to query that in the future.

```javascript
$("#my-list li").each(function () {
  let li = $(this);
  let div = li.find("div.content");
  li.data("contentDiv", div);
});
// later
let firstLi = $("#my-list li:first");
firstLi.data("contentDiv").html("new content");
```

# Events

## Adding event handlers

```javascript
$("p").last().on("click", (e) => { ... })
```

## Triggering events

```javascript
$(".button").trigger("click");
```

# JQuery Ajax

## Deferred object

JQuery implements their own `Deferred` object where `Promise` is the interface.
So it has a superset of fields and methods of `Promise`.

## Sending HTTP requests

Use **"minified"** version. JQuery's Ajax methods are not provided in "slim
minified" version.

JQuery directly transforms JSON into JavaScript object for you.

```javascript
$.ajax({
  url: "http://api.open-notify.org/astros.json",
  method: "GET",
})
  .done((data) => {
    console.log(data);
  })
  .fail((error) => {
    console.log(error);
  })
  .always((data) => {
    console.log("...");
  });
```

Besides `$.ajax()`, there are `$.get()`, `$.post()` and etc.

## Handling multiple deferred objects

`$.when(deferred...)` takes in multiple Deferred object and return a Promise
that resolve into a resolved Deferred if all input Deferred resolve, or into a
rejected Deferred as soon as any input rejects.

You can chain Deferred methods.

# JQuery Form Helper Functions

A jQuery form element has a method `serialize()` that takes the values of the
form and output a corresponding URL query string:

```javascript
$("#form").serialize();
// "name=John&email=john@example.com&tel=12345678"
```

The method `serializeArray()` outputs an array of key-value pairs:

```javascript
$("#form").serializeArray();
/* [
 *   {name: "name", value: "John"},
 *   {name: "email", value: "john@example.com"},
 *   {name: "tel", value: "12345678"}
 * ]
 */
```

# 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](.index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../index.md)
- [🗃️ Master Index](../../../index.md)
