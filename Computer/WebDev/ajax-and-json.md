---
date: 2024-05-29 (Wed)
---

# AJAX and JSON

## Overview

**_Asynchronous JavaScript with XML_** is a paradigm that allows a web browser
to make quick, **incremental updates** to the web page without reloading the
entire page.

## XMLHttpRequest Object

**_XMLHttpRequest object_** (short for **_XHR object_**) is the core of AJAX.

1. Create an XMLHttpRequest object
2. Register a callback function to the `onreadystatechange` property of the XHR
   object
3. Use the `open()` method to defines request format and content. And set
   appropriate headers with `setRequestHeader()` method.
4. Use the `send()` method to send the request to the server. Specify in the
   argument the type of request (e.g. GET or POST) and whether asynchronously
   (default).
5. When the response arrives, the callback function is triggered.
6. Usually update the web page by DOM manipulation.

```javascript
let httpRequest;

if (window.XMLHttpRequest) {
  httpRequest = new XMLHttpRequest();
} else if (window.ActiveXObject) {
  // IE 6 and older
  httpRequest = new ActiveXObject("Microsoft.XMLHTTP");
} else {
  // browser doesn't support AJAX
}
```

### open method

`open(method: Method, url: string, async = true, user?: string, password?: string)`

Methods include GET, POST, PUT, DELETE, etc.

### send method

`send()` for most methods, `send(string)` for POST

To use POST to send form data, you may set the "Content-Type" header:

```javascript
httpRequest.open("POST", "process.php", true);
httpRequest.setRequestHeader(
  "Content-Type",
  "application/x-www-form-urlencoded",
);
httpRequest.send("name=value");
```

## Fetch API

### Intro to fetch

It is a native JavaScript API. Default is GET.

```javascript
fetch(url).then(/* callback function */).catch(/* callback function */);
```

`fetch()` returns a Promise object, which resolves to a **_Response_** object
eventually.

### Response object

| Property     | Description                                     |
| ------------ | ----------------------------------------------- |
| `status`     | HTTP response code (100 - 599)                  |
| `statusText` | Status text                                     |
| `ok`         | True if status is HTTP 2xx                      |
| `headers`    | The Headers object associated with the response |
| `url`        | The URL of response                             |
| `type`       | E.g. basic, cors                                |
| `redirected` | Whether reponse is result of redirection        |

The following methods return a **promise** that eventually resolved to different
result. Since it is a promise, you need to chain further `then()` to handle
their resolved return value.

| Method          | Resolved Result                   |
| --------------- | --------------------------------- |
| `text()`        | body as a string                  |
| `json()`        | JS object by parsing body as JSON |
| `blob()`        | body as a Blob object             |
| `arrayBuffer()` | body as an ArrayBuffer object     |
| `formData()`    | body as a FormData object         |

### Headers Object and Request Object

Headers:

- Represents response/request headers
- Allow you to query them and take different actions depending on the results
- Create your own headers before sending the `fetch()` request

Request:

- Represents a resource request
- We can manually compose a request object that uses POST method and customize
  body and pass the Request object to `fetch()`

### Options of fetch

To customize the fetch request in `fetch(url, init)`, you need to provide an
**_init object_** as the second argument with the following options:

- `method`
- `headers`
- `body`
- `cache`
- `referrer`, `keepalive`, `credentials`, `mode`, ...

```javascript
const init = {
  method: "POST",
  body: "number=" + studentNum.value,
  headers: { "Content-Type": "application/x-www-form-urlencoded" },
};
fetch("checkPost.php", init)
  .then((res) => res.text())
  .then((data) => {
    /* handle data */
  });
```

## Promise Object

See [Promises and AJAX](JavaScript/promises-and-ajax.md).

## Async Await

To write asynchronous operation in the syntax of writing synchronous functions.

```javascript
async function fetchRequest() {
  try {
    let response = await fetch("checking.php?number=" + snum.value);
    if (response.status == 200) {
      let data = await response.text();
      // handle data
    } else {
      console.log("HTTP return status: " + response.status);
    }
  } catch (err) {
    console.log("Fetch Error!");
  }
}
```

- Note that `async` keyword to mark the function as asynchronous
- The keyword `await` can only be used in an `async` function
  - And it **cannot** be used in a **nested** (arrow / anonymous) function that
    is not asynchronous.
- `await` takes a promise, and when the promise returns, `await` turns it into a
  resolved **response** or **error**.
- Note that when using `.json()` of a response, you need to `await` because
  `.json()` doesn't return a JavaScript object directly, but a promise.
- `await` statement is executed **synchronously** within a function. So any
  statements after that has to wait until the `await` statement finishes. It is
  only the `async` function that is **called asynchronously**.

## JSON

### Intro to JSON

**_JavaScript Object Notation_**. A plain text format for representing
structured data based on **JS object syntax**.

### JSON Syntax

There are few difference with the modern JS syntax:

- Key of an object is a string and must be enclosed by **double** quotes
- No trailing commas are allowed after the last field of object or last element
  of array
- A JSON object is purely a data format, only containing properties but no
  methods.

### JSON Value Types

JSON data is written in name-value pairs. Values can be one of six simple data
types:

- strings: must be enclosed by **double quotes**
- numbers
- objects: circumscribed by curly braces
- arrays: circumscribed by square brackets
- booleans: `true` or `false`
- `null` or empty.

### Using JSON in JavaScript

- `JSON.stringify(jsObject)` returns a string in JSON format.
  - It optionally accepts a replacer function to replace values:
    `JSON.stringify(jsObject, replacerFunc(key, value))`
- `JSON.parse(jsonString)` returns a JavaScript object.
  - Optionally accepts a reviver function to perform a transformation on the
    sulting object before it is returned, like `stringify()`

```javascript
const jsonString = '{"name": "Kenneth", "id": [123, 456]}';
const jsObject = { name: "Kenneth" };
```

## 🧭 Navigation

- [🔼 Back to top](#ajax)
- [◀️ Back](../../index.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
