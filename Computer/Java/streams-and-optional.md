---
date: 2024-08-06 (Tue)
---

# Streams and Optional

## Transforming into a Stream

```java
myList.stream();
DoubleStream.of(1.0, 2.0, 3.0);
IntStream.of(myIntArray);
```

## Intermediate Operations

- `map()`
  - `mapToInt()`, `mapToLong()`, `mapToDouble()`
  - Using specific type of map allows you to have more terminal operations
- `filter()`

## Terminal Operations

- `count()`
- `collect()`
  - E.g. `.collect(Collectors.toList())`
- `max()`
- `min()`
- `sum()`
- `average()`
- `forEach()`
- `reduce()`
  - `reduce(accumulator)`: if you only pass in an accumulator, it returns an
    `Optional`, which is an empty `Optional` if the stream is empty. It is also
    smart enough to not run the accumulator for the first element, but only
    starting from the second.
  - `reduce(initialValue, accumulator)`

## 🧭 Navigation

- [🔼 Back to top](#streams-and-optional)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../index.md)
