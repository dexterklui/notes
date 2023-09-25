---
title: Local Storage
---

```javascript
// Store a json string into local storage with the key "userInfo"
window.localStorage.setItem("userInfo", JSON.stringify(user1));
// Retrieve from local storage with the key "userInfo"
const userInfo = JSON.parse(window.localStorage.getItem("userInfo"));
```
