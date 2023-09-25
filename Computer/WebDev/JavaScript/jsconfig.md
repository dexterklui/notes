---
title: jsconfig
---

# Basic Setup for JavaScript

```json
{
  "compilerOptions": {
    "target": "ES2017",
    "lib": ["ES2017", "DOM", "DOM.Iterable"],
    "checkJs": true
  }
}
```

- `checkJs` enables type checking on JavaScript file
- `lib` DOM.Iterable says that NodeList is iterable. Otherwise it is only
  iterable with basic for loop and forEach loop, but not for of loop.

# JQuery

To enable auto-completion and types defined in jQuery, add the following config.

```json
{
  "typeAcquisition": {
    "include": ["jquery"]
  }
}
```
