---
title: Tips and Tricks
date: 2023-09-23 (Sat)
---

# Return and Handle Different Types Conditionally

## Problem

Sometimes a function returns different kinds of result in different situations.
And outside you need to handle the returned value differently based on the kinds
of result returned.

One basic example is in try-catch block, you may want to return a meaningful
string normally but an error string in case of an error or some special
occasion. Since they are both strings, so native type checking doesn't work. In
TypeScript, you can create custom types to help you identify what kinds of
result is returned, but not in JavaScript. But actually there is a much more
simplistic method to solve this problem.

## Solution

The solution is to wrap the result in an object before returning. Different
kinds of result uses different keys. This way, you can use destructuring in the
outer scope to put the result into the variable that represent a particular kind
of result.

```js
async function getData() {
  try {
    const res = await fetch("https://example.com/unreliable/api");
    const data = await res.json();

    return { data };
  } catch (error) {
    return { error: "Could not fetch data." };
  }
}

const { data, error } = await getData();
if (error) {
  // handle error...
}
```

# 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../../index.md)
- [🗃️ Master Index](../../../../index.md)
