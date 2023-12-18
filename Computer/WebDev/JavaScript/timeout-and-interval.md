---
title: Timeout and Interval
date: 2023-10-27 (Fri)
---

# Clearing Scheduled Tasks

This is especially important for **intervals** to prevent memory leak.

`setTimeout` and `setInerval` either return a numerical ID (in a **browser**) or
a reference to the created **scheduler object** (in Node.js). Their returned
values can be passed to `clearInterval` and `clearTimeout`, respectively, to
remove the scheduler. After removing the scheduler, the scheduled task will not
be executed.

You can either store their returned value in a variable and remove the scheduler
later using that. Or on a **server side** (Node.js) you can remove the scheduler
in the **anonymous (not arrow function)** callback function by referencing the
scheduler through the keyword `this`, like the following:

```javascript
let count = 0;
setInterval(function () {
  console.log(++count);
  if (count >= 10) clearInterval(this);
}, 1000);
```

# 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../../index.md)
- [🗃️ Master Index](../../../../index.md)
