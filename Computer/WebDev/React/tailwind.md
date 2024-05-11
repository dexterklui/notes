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

## Class Name Merges

See
[How to Merge Class Names in Tailwind and React](https://korayguler.medium.com/how-to-merge-react-and-tailwind-css-class-names-f5faeb10ed24)

Basically you use two packages:

- `clsx`: convenient to add classes conditionally
- `tailwind-merge`: merge conflicting tailwind classes

With these two packages, you export a function `cn`:

```typescript
import { ClassValue, clsx } from "clsx";
import { twMerge } from "tailwind-merge";

export const cn = (...inputs: ClassValue[]) => {
  return twMerge(clsx(inputs));
};
```

And use it to merge class names:

```javascript
import cn from "./utils/cn";

const Button = (props) => {
  return (
    <button className={cn("btn", "bg-red-500", props.className)}>
      Click me
    </button>
  );
};

export default Button;
```

## 🧭 Navigation

- [🔖 Parent index](../../../index.md)
- [📑 Notes Index](../../../index.md)
