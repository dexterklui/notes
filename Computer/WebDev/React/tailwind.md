---
date: 2023-12-18 (Mon)
---

# Tailwind

## Idiosyncrasy

### Dynamic Class Name Does Not Work

Tailwind doesn't include any sort of client-side runtime, so class names need to
be statically extractable at build-time, and can't depend on any sort of
arbitrary dynamic values that change on the client.

E.g. ``className={`bg-${color}-500`}`` won't work.

See this
[StackOverflow](https://stackoverflow.com/questions/69687530/dynamically-build-classnames-in-tailwindcss).

## 🧭 Navigation

- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
