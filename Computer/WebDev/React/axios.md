---
date: 2024-08-29 (Thu)
---

# Axios

```javascript
axios
  .get("https://api.example.com")
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.log(error);
  });
```

Using in useEffect:

```js
const url = "https://api.example.com";
const body = { name: "John" };
const reqOptions = {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify(body),
};
axios
  .get(url)
  .then((response) => response.data)
  .catch((error) => console.log(error));
```

## 🧭 Navigation

- [🔼 Back to top](#axios)
- [◀️ Back](../../../index.md)
- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
