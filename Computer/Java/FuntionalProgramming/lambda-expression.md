---
date: 2024-07-30 (Tue)
---

# Lambda Expression

## Functional Programming

### Languages

- Haskel
- Lisp

## Functional Interface

Interfaces marked by annotation `@FunctionalInterface` can be used as the
assignment target for a lambda expression or method reference.

A functional interface can only declare one abstract method.

```java
@FunctionalInterface
public interface MyFunctionalInterface {
  int apply(int x);
}

MyFunctionalInterface incrementor = x -> x + 1;
final int num = 0;
final int numIncremented = incrementor.apply(num);
```

`Callable` and `Runnable` are examples of functional interfaces.

## Built-In Functional Interface

- `IntBinaryOperator` declares `int applyAsInt(int x, int y)`
- `BinaryOperator<T>` declares `T apply(T left, T right)`
- `Function<T, R>` takes in one argument of any type and return one argument of
  any type. It declares `R apply(T t)`.
- `BiFunction<T, U, R>` declares `R apply(T t, U u)`
- `Predicate<T>` declare `boolean test(T t)`
- `Consumer<T>` declares `void accept(T t)`
- `Producer<T>` declares `T get()`

## Common methods using lambda expression

- Map.merge
- Collection.forEach
- Collection.removeIf
- Collection.replaceAll

## Helper methods

Combining or creating functional interfaces:

- Predicate has `and()`, `or()` to combine with another predicate, or `negate()`
  methods.
- Compartor has `thenComparing()` method.

## 🧭 Navigation

- [🔼 Back to top](#lambda-expression)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../../index.md)
