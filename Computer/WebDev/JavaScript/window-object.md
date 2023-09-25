---
title: Window Object
date: 2023-08-06 (Sun)
---

# Methods

- `open(url)`. With `"_blank"` as a second argument, open in a new tab.

# Properties

- `innerWidth`
- `innerHeight`

# Storage

## Session Storage

`sessionStorage` data gets cleared when the page session ends.

## Local Storage

- `Window.localStorage`
- Data has no expiration time (except in "incognito session", where data is
  cleared after the last "private" tab is closed)
- Keys and values are always in UTF-16 string (numbers are converted to
  strings).
- Stored data is **specific to protocol**. E.g. HTTP and HTTPS to the same
  location uses different data. (`file:` protocol has no fixed behaviour across
  browsers)

```javascript
// Store a json string into local storage with the key "userInfo"
window.localStorage.setItem("userInfo", JSON.stringify(user1));
// Retrieve from local storage with the key "userInfo"
const userInfo = JSON.parse(window.localStorage.getItem("userInfo"));
// Remove an item. Skip `window` is ok when in global scope
localStorage.removeItem("userInfo");
// Remove all items
window.localStorage.clear();
```

# 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../index.md)
- [🗃️ Master Index](../../../index.md)
