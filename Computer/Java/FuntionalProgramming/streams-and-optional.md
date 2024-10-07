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

  - Use builtin collector functions e.g. `.collect(Collectors.toList())`
    - `Collectors.toMap(Function.identity(), x -> String.valueOf(x).toUpperCase())`
      - The first argument is a function that returns the key, the second
        returns the value
    - `Collectors.joining(", ")` joins the element with a delimiter
  - `<R> R collect(Supplier<R> supplier, BiConsumer<R, ? super T> accumulator,BiConsumer<R, R> combiner)`
    - `supplier`: a function that creates a new mutable result container. For
      the parallel execution, this function may be called multiple times and it
      must return a fresh value each time.
      - You can also use e.g. `ArrayList::new`
    - `accumulator` is a stateless function that must fold an element into a
      result container.
      - You can also use e.g. `ArrayList::add`
    - `combiner` is a stateless function that accepts two partial result
      containers and merges them, which must be compatible with the accumulator
      function.
      - You can also use e.g. `ArrayList::addAll`
      - Only used for parallel streams (with `Collection.parallelStream()`)

- `max()` relies on the `compareTo()` method of the object
- `min()` relies on the `compareTo()` method of the object
- `forEach()`
- `reduce()`
  - `reduce(accumulator)`: if you only pass in an accumulator, it returns an
    `Optional`, which is an empty `Optional` if the stream is empty. It is also
    smart enough to not run the accumulator for the first element, but only
    starting from the second.
  - `reduce(initialValue, accumulator)`
- `sum()` - only available on `IntStream`, `LongStream`, and `DoubleStream`
- `average()` - only available on `IntStream`, `LongStream`, and `DoubleStream`

## 🧭 Navigation

- [🔼 Back to top](#streams-and-optional)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../../index.md)
