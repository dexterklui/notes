---
title: Basics
date: 2023-07-26 (Wed)
---

# Data Types

## Dynamically and weakly typed

- **Dynamically typed**: type of variables are determined at runtime
- **Weakly typed**: type of variables can be implicitly inferred for operations
  between different data types.

## String

### Instance methods

The argument `s` means a string.

| Method             | Description                               |
| ------------------ | ----------------------------------------- |
| `padStart(len)`    | Prepend spaces until target length        |
| `padStart(len, s)` | Use s as pad string; truncate s if needed |
| `padEnd(len)`      | Append spaces until target length         |
| `padEnd(len, s)`   | Use s as pad string; truncate s if needed |

# Hoisting

**_Hoisting_** is JavaScript's default behaviour of moving all declarations to
the **top** of the current scope. Note that hoisted variables are only declared
but **not initialized**.

But as a good practice, you should declare all variables at the top of the
scope. That can also prevent beginners from misunderstanding.

# 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../index.md)
- [🗃️ Master Index](../../../index.md)
